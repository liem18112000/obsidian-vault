---
title: "Throw error codes not sentences; localise at the request boundary"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Handle error exception (LUZFIN)"
tags: [error-handling, i18n, jax-rs, exception-mapper, java, confluence-distilled]
---

# Throw error codes not sentences; localise at the request boundary

An exception thrown deep in a service layer does not know the caller's language. If it carries a pre-formatted English sentence, that sentence is what the user gets — or you end up doing string matching at the edge to re-translate it.

Carry a **code plus its bundle and parameters** instead, and resolve the text at the HTTP boundary where the request's locale is known:

```java
public class LocalizedException extends Exception {
    private String code;                 // e.g. "document.not.found"
    private String resourceBundlePath;   // which bundle owns that code
    private Object[] params;             // values interpolated into the message

    public LocalizedException(String code, String resourceBundlePath, Object... params) { … }

    public String getLocalizedMessage(Locale preferredLanguage) {
        return ErrorMessageBundle
            .byPath(this.resourceBundlePath)
            .inClassPath(this.getClass().getClassLoader())
            .build()
            .get(code, preferredLanguage, params);
    }
}
```

A JAX-RS `@Provider` then does the rendering, pulling the locale from the live request:

```java
@Provider
public class LocalizedExceptionMapper implements ExceptionMapper<LocalizedException> {
    @Inject private HttpServletRequest httpServletRequest;
    // → resolve Accept-Language, call getLocalizedMessage(locale), build the response
}
```

**What the split buys:**

- **Business code stays language-free.** A service throws `new LocalizedException("document.not.found", BUNDLE, id)`. It never imports a locale, and it never changes when a new language is added.
- **The resolution point knows the locale.** Only the boundary has the request, and therefore `Accept-Language`. That is the one place capable of choosing the right text.
- **The code is a stable contract; the text is not.** Clients can branch on `document.not.found` and it keeps working when someone rewords the German copy. A client matching on message text breaks silently on a translation edit.
- **Parameters interpolate per language.** Passing `params` separately lets each translation place them where its grammar requires, rather than concatenating fragments in a fixed order.

> [!tip] Pair it with a second mapper for everything else
> The same design has a `SystemExceptionMapper` alongside the localized one. That division is the useful part: **expected, business-rule failures** get a localized, user-facing message; **unexpected** ones get a generic message plus a correlation id, with the detail going to logs and not to the user. Two mappers, two audiences.

> [!warning] A missing bundle key must not throw
> Resolution happens while handling an error, so a `MissingResourceException` there replaces a useful 4xx with an opaque 500 — the worst possible time to fail. Fall back to the code itself, or to a default locale, and treat missing keys as a build-time check rather than a runtime surprise.

Related: [[Bulk operations need per-item outcomes, not one status code]] — the other half of designing error responses.

Source: [[Handle error exception]] (LUZFIN, Confluence).

## Related

- [[Bulk operations need per-item outcomes, not one status code]]
