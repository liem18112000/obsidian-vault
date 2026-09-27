---
title: "Java's options for options"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47173010019/Java+s+options+for+options
space: "TS"
topic: programming
relevance: 0.792
depth: 3
updated: 2022-08-28
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Java's options for options

> [!info] Imported from Confluence
> Space **TS** · updated 2022-08-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47173010019/Java+s+options+for+options)
> Relevance 0.792 · topic `programming`

by: **Ethan McCue**

# Hypothetical 1

Imagine that you made a bit of code that outputs JSON.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b66517a2-6491-478f-9fea-d7034bf2c9b7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json
    ) {
        ...
    }
}
```

</div>

</div>

By default, your output contains no extra whitespace, but you want to provide an option to the user to print that JSON with some indentation.

### Without indentation

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e861b9c1-0480-4b5f-94a2-14458ca6faec" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[{"name":"joe","age":35}]
```

</div>

</div>

### With indentation

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ff119fc6-f34d-41a5-a84a-41cebfe7252d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
    {
        "name":"joe",
        "age":35
    }
]
```

</div>

</div>

## Option 1. Don't support it

Any toggles you add to your API are toggles you might need to support now and forever more. Depending on the code you are writing and who its consumers are, it might make more sense to provide a more restricted API.

## Option 2. Make another method

With only a single option you want configurable, you can just expose a method with a different name.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="839baf0d-2be4-4e46-b2ea-61510dd8a938" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonWithIndentation(
            Appendable out,
            Json json
    ) {
        ...
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="38cf0f56-c392-4ff9-853f-a2ad45c98235" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json);
writeJsonWithIndentation(out, json);
```

</div>

</div>

## Option 3. Add a boolean argument

A single option is either on or off. True or false. That is often the domain of a boolean.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4c1ec130-131d-49b4-9823-da237d40c763" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json,
            boolean indent
    ) {
        if (indent) {
            ...
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="486a075e-638b-4b0b-a956-10d0f95a1f66" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, true);
writeJson(out, json, false);
```

</div>

</div>

## Option 4. Add an enum argument

Booleans are great, but for understandability, at the call site you might want to provide an enum with two possible values instead.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b86704d9-34d2-45d0-8c85-daa41ae70669" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indentation {
    INDENT,
    DO_NOT_INDENT
}

...

public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json,
            Indentation indent
    ) {
        switch (indent) {
            case INDENT -> ...
            case DO_NOT_INDENT -> ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c560c4ba-49f3-49e8-bd5f-857c8f4b1ada" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, Indentation.INDENT);
writeJson(out, json, Indentation.DO_NOT_INDENT);
```

</div>

</div>

------------------------------------------------------------------------

# Hypothetical 2

Say now you get some feedback that while the indentation style is great for objects, it is sometimes not great for JSON with long arrays.

## No indentation

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="767baff5-b097-4403-adc6-1dd4f2cae151" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[{"numbers":[1,2,3]}]
```

</div>

</div>

### Indent Everything

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7f7d5c21-b597-47c9-ab24-eb2d89685802" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {
    "numbers": [
        1,
        2,
        3
    ]
  }
]
```

</div>

</div>

### Indent Objects

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1085080e-5655-4df8-8cc6-19cb9eeaeb0e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[{
    "numbers": [1, 2, 3]
}]
```

</div>

</div>

### Indent Arrays

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6bf2fedb-ef19-4ca7-9f41-712219aa0e9d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {"numbers": [
        1,
        2,
        3
  ]}
]
```

</div>

</div>

## Option 5. Make methods for requested combinations

Your users just want a way to turn off indentation for arrays. Give it to them.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ff2d510e-b348-4963-9ad7-5b4badeafc0f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentObjects(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentEverything(
            Appendable out,
            Json json
    ) {
        ...
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="06311ca0-a33e-4c04-a3a2-2b47d9f210d3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json);
writeJsonIndentObjects(out, json);
writeJsonIndentEverything(out, json);
```

</div>

</div>

## Option 6. Make methods for every combination

There are four logical settings that come out of two different flags, so you can certainly provide all four options as methods. Could save you time later.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0cf27eb2-54db-436d-9d82-83927d746b58" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentObjects(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentArrays(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentEverything(
            Appendable out,
            Json json
    ) {
        ...
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="edd87b59-9f29-4d9a-bfa6-929373eacc1c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json);
writeJsonIndentObjects(out, json);
writeJsonIndentArrays(out, json);
writeJsonIndentEverything(out, json);
```

</div>

</div>

## Option 7. Have two boolean arguments

Two logically independent things to configure, you can always take a boolean for each.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="25ed0b4a-d8b1-49f6-b656-13ea2826b9ea" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json,
            boolean indentObjects,
            boolean indentArrays
    ) {
        if (indentObjects) {
            if (indentArrays) {
                ...
            }
            else {
                ...
            }
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="022950b2-4b0c-440d-a58f-b96059b4312b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, true, true);
writeJson(out, json, true, false);
writeJson(out, json, false, true);
writeJson(out, json, false, false);
```

</div>

</div>

## Option 8. Have two enum arguments

Booleans describe everything, but enums are still more explicit.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="80800b6b-8853-4475-83aa-29bc10ff7cd0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indentation {
    INDENT,
    DO_NOT_INDENT
}

...

public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json,
            Indentation indentObjects,
            Indentation indentArrays
    ) {
        switch (indentObjects) {
            case INDENT -> switch (indentArrays) {
                case INDENT -> ...
                case DO_NOT_INDENT -> ...
            }
            case DO_NOT_INDENT -> ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="352e7757-b7d5-4e27-bdc6-21f8cdd5e023" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, Indentation.INDENT, Indentation.INDENT);
writeJson(out, json, Indentation.INDENT, Indentation.DO_NOT_INDENT);
writeJson(out, json, Indentation.DO_NOT_INDENT, Indentation.INDENT);
writeJson(
        out, 
        json, 
        Indentation.DO_NOT_INDENT, 
        Indentation.DO_NOT_INDENT
);
```

</div>

</div>

## Option 9. Take options as bit flags

It's an old-school solution and maybe a bit too clever, but you are feeling old school and clever.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bcd35c35-3b82-4487-bc02-996331657449" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class Indentation {
    public static final int NO_INDENTATION = 0b00;
    public static final int INDENT_OBJECTS = 0b01;
    public static final int INDENT_ARRAYS = 0b10;
    
    private Indentation() {}
}

...

public final class JsonWriter {
    private JsonWriter() {}
    
    public static void writeJson(
            Appendable out,
            Json json,
            int indentation
    ) {
        if ((indentation & Indentation.INDENT_OBJECTS) != 0) {
            if ((indentation & Indentation.INDENT_ARRAYS) != 0) {
                ...
            }
            else {
                ...
            }
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="10b81b7b-5162-428a-a922-18530c1c795c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
        out, 
        json, 
        Indentation.INDENT_OBJECTS | Indentation.INDENT_ARRAYS, 
);
writeJson(out, json, Indentation.INDENT_OBJECTS);
writeJson(out, json, Indentation.INDENT_ARRAYS);
writeJson(out, json, Indentation.NO_INDENTATION);
```

</div>

</div>

## Option 10. Take an EnumSet

Rather than waste a parameter on each flag, explicitly take the set of behaviors they want to enable.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8a740689-435f-442b-8d61-570a18fa7cb2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indent {
    OBJECTS,
    ARRAYS
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            EnumSet<Indent> indent
    ) {
        if (indent.contains(Indent.OBJECTS)) {
            if (indent.contains(Indent.ARRAYS)) {
                ...
            }
            else {
                ...
            }
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1541696d-cebc-464a-9ac9-1665430b3b31" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, EnumSet.of(Indent.OBJECTS, Indent.ARRAYS));
writeJson(out, json, EnumSet.of(Indent.OBJECTS));
writeJson(out, json, EnumSet.of(Indent.ARRAYS));
writeJson(out, json, EnumSet.noneOf(Indent.class));
```

</div>

</div>

## Option 11. Take a transparent config object

Similar to just taking two booleans, putting them in an object means you can refer to a set of options as a concrete "thing". This could help you keep the most common usages terse.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="65124334-e5b2-4e56-ac08-a9bf99b1ba7b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
record Options(boolean indentObjects, boolean indentArrays) {
    public static final Options INDENT_EVERYTHING =
            new Options(true, true);
    public static final Options NO_INDENT =
            new Options(false, false);
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            Options options
    ) {
        if (options.indentObjects()) {
            if (options.indentArrays()) {
                ...
            }
            else {
                ...
            }
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f0a94cf0-0456-4102-8cf8-8cb3d094c7ca" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, Options.INDENT_EVERYTHING);
writeJson(out, json, new Options(true, false));
writeJson(out, json, new Options(false, true));
writeJson(out, json, Options.NO_INDENT);
```

</div>

</div>

## Option 12. Take an opaque config object

Maybe you want to give your API some extra wiggle room to grow. Maybe you just like how the usage of an opaque object made from a builder looks.

With this approach, you can choose to internally represent things as booleans, enums, an enum set, bit flags, or whatever other evil lies within the hearts of mankind.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d1b44326-bf54-4aa2-9979-7f8f62fe46c8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class Options {
    private final boolean indentObjects;
    private final boolean indentArrays;

    private Options(Builder builder) {
        this.indentArrays = builder.indentArrays;
        this.indentObjects = builder.indentObjects;
    }

    public boolean indentArrays() {
        return this.indentArrays;
    }

    public boolean indentObjects() {
        return this.indentObjects;
    }

    public static Options standard() {
        return builder().build();
    }
    
    public static Builder builder() {
        return new Builder();
    }

    public final class Builder {
        private boolean indentObjects;
        private boolean indentArrays;

        private Builder() {
            this.indentObjects = false;
            this.indentArrays = false;
        }
        
        public Builder indentObjects() {
            this.indentObjects = true;
            return this;
        }
        
        public Builder indentArrays() {
            this.indentArrays = true;
            return this;
        }
        
        public Options build() {
            return new Options(this);
        }
    }
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            Options options
    ) {
        if (options.indentObjects()) {
            if (options.indentArrays()) {
                ...
            }
            else {
                ...
            }
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b0ea9151-9194-4000-8063-8d3ea42f0f09" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
    out, 
    json, 
    Options.builder()
        .indentObjects()
        .indentArrays()
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .indentObjects()
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .indentArrays()
        .build()
);
writeJson(out, json, Options.standard());
```

</div>

</div>

------------------------------------------------------------------------

# Hypothetical 3.

🚑 Weewoo Weewoo 🚑

It's legal! Your APIs are great and all, but when you send data to external clients we would really like to include an explicit statement of copyright. That copyright message might change depending on your contract with the client and also we shouldn't send it internally.

Good luck, legal out.

### With Indentation, Without copyright

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f5c513c-2341-488c-8c0d-ed8983927d6f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
    {
        "name": "joe",
        "age": 35
    }
]
```

</div>

</div>

### With indentation, With copyright

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="759b113b-7275-4f2f-8c7b-69e356007f1b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{ 
    "copyright": "(c) 2022 Inc.",
    "data": [
        {
            "name":"joe",
            "age":35
        }
    ]
}
```

</div>

</div>

## Option 13. Add 4 more methods to hit the new combinations

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7041e0f2-0e39-4564-bd82-8fc3cc20d2d4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentObjects(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentArrays(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonIndentEverything(
            Appendable out,
            Json json
    ) {
        ...
    }

    public static void writeJsonWithCopyright(
            Appendable out,
            Json json,
            String copyright
    ) {
        ...
    }

    public static void writeJsonIndentObjectsWithCopyright(
            Appendable out,
            Json json,
            String copyright
    ) {
        ...
    }

    public static void writeJsonIndentArraysWithCopyright(
            Appendable out,
            Json json,
            String copyright
    ) {
        ...
    }

    public static void writeJsonIndentEverythingWithCopyright(
            Appendable out,
            Json json,
            String copyright
    ) {
        ...
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f338788c-56b1-47bc-9036-732dfbe1ecc9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json);
writeJsonIndentObjects(out, json);
writeJsonIndentArrays(out, json);
writeJsonIndentEverything(out, json);
writeJsonWithCopyright(out, json, "(c) 2022");
writeJsonIndentObjectsWithCopyright(out, json, "(c) 2022");
writeJsonIndentArraysWithCopyright(out, json, "(c) 2022");
writeJsonIndentEverythingWithCopyright(out, json, "(c) 2022");
```

</div>

</div>

## Options 14. Add a single new method

If your boolean-like options only took up a single overload, you can get away with just adding a single new method to the list.

This will look different depending on whether you used booleans, enums, an `EnumSet`, or bit flags.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0f458574-b463-42bf-a46c-7aa944bd5d28" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indent {
    OBJECTS,
    ARRAYS
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            EnumSet<Indent> indent
    ) {
        
    }

    public static void writeJson(
            Appendable out,
            Json json,
            EnumSet<Indent> indent,
            String copyright
    ) {

    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5993d860-fff1-46cc-8f21-b29cadf61b9c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, EnumSet.of(Indent.OBJECTS, Indent.ARRAYS));
writeJson(out, json, EnumSet.of(Indent.OBJECTS));
writeJson(out, json, EnumSet.of(Indent.ARRAYS));
writeJson(out, json, EnumSet.noneOf(Indent.class));
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.OBJECTS, Indent.ARRAYS),
    "(c) 2022"
);
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.OBJECTS),
    "(c) 2022"
);
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.ARRAYS),
    "(c) 2022"
);
writeJson(
    out, 
    json, 
    EnumSet.noneOf(Indent.class), 
    "(c) 2022"
);
```

</div>

</div>

## Option 15. Add another argument and accept null

If you don't want to add yet another overload, you can always just allow users to pass `null`.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c586af18-98bb-48d2-9925-515dd875a2e0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indent {
    OBJECTS,
    ARRAYS
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            EnumSet<Indent> indent,
            String copyright
    ) {
        if (indent.contains(Indent.OBJECTS)) {
            if (indent.contains(Indent.ARRAYS)) {
                if (copyright == null) {
                    ...
                }
                else {
                    ... 
                }

            }
            else {
                ...
            }
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8e3cdc91-0759-4bfa-af75-e244069ae45f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(out, json, EnumSet.of(Indent.OBJECTS, Indent.ARRAYS), null);
writeJson(out, json, EnumSet.of(Indent.OBJECTS), null);
writeJson(out, json, EnumSet.of(Indent.ARRAYS), null);
writeJson(out, json, EnumSet.noneOf(Indent.class), null);
writeJson(
        out,
        json,
        EnumSet.of(Indent.OBJECTS, Indent.ARRAYS), 
        "(c) 2022"
);
writeJson(out, json, EnumSet.of(Indent.OBJECTS), "(c) 2022");
writeJson(out, json, EnumSet.of(Indent.ARRAYS), "(c) 2022");
writeJson(out, json, EnumSet.noneOf(Indent.class), "(c) 2022");
```

</div>

</div>

## Option 16. Add another argument and make it an `Optional`

It's time to paint a bike shed. You can do this with `java.util.Optional` or your own sealed type.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9e8cd010-bd08-4d01-8c9b-9bba1c4eed87" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indent {
    OBJECTS,
    ARRAYS
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            EnumSet<Indent> indent,
            Optional<String> copyright
    ) {
        if (indent.contains(Indent.OBJECTS)) {
            if (indent.contains(Indent.ARRAYS)) {
                if (copyright.isPresent()) {
                    ... copyright.orElseThrow() ...
                } else {
                    ...
                }
            } else {
                ...
            }
        } else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e606012c-1a49-42d9-8491-a40da90bc218" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.OBJECTS, Indent.ARRAYS), 
    Optional.empty()
);
writeJson(
    out,
    json, 
    EnumSet.of(Indent.OBJECTS),
    Optional.empty()
);
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.ARRAYS),
    Optional.empty()
);
writeJson(
    out, 
    json, 
    EnumSet.noneOf(Indent.class), 
    Optional.empty()
);
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.OBJECTS, Indent.ARRAYS), 
    Optional.of("(c) 2022")
);
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.OBJECTS), 
    Optional.of("(c) 2022")
);
writeJson(
    out, 
    json, 
    EnumSet.of(Indent.ARRAYS), 
    Optional.of("(c) 2022")
);
writeJson(
    out, 
    json, 
    EnumSet.noneOf(Indent.class), 
    Optional.of("(c) 2022")
);
```

</div>

</div>

## Option 17. Add a nullable property to a transparent config object

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6a4f6f67-3e93-4aff-89b2-9bbef5427e06" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
record Options(
        boolean indentObjects, 
        boolean indentArrays,
        String copyright) {
    public static final Options INDENT_EVERYTHING =
            new Options(true, true, null);
    public static final Options NO_INDENT =
            new Options(false, false, null);
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            Options options
    ) {
        if (options.indentObjects()) {
            if (options.indentArrays()) {
                if (options.copyright() == null) {
                    ...
                }
                else {
                    ...   
                }
            }
            else {
                ... 
            }
        }
        else { 
            ... 
        }
    }
}
```

</div>

</div>

## Option 18. Add an `Optional` property to a transparent config object

Same as above, but if you like to explicitly have the property be optional you can, but if you are using records as your transparent data carriers then you need to take an `Optional` in your constructor.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dd292c5e-809f-42c0-b5da-83e844d93088" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
record Options(
        boolean indentObjects,
        boolean indentArrays,
        Optional<String> copyright
) {
    public static final Options INDENT_EVERYTHING =
            new Options(true, true, Optional.empty());
    public static final Options NO_INDENT =
            new Options(false, false, Optional.empty());
}
```

</div>

</div>

## Option 19. Add another property to an opaque config object

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a8675e67-3d91-49cb-b287-10b4837d1edc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class Options {
    private final boolean indentObjects;
    private final boolean indentArrays;
    private final String copyright;

    private Options(Builder builder) {
        this.indentArrays = builder.indentArrays;
        this.indentObjects = builder.indentObjects;
        this.copyright = builder.copyright;
    }

    public boolean indentArrays() {
        return this.indentArrays;
    }

    public boolean indentObjects() {
        return this.indentObjects;
    }

    public Optional<String> copyright() {
        return Optional.ofNullable(this.copyright);
    }

    public static Options standard() {
        return builder().build();
    }

    public static Builder builder() {
        return new Builder();
    }

    public final class Builder {
        private boolean indentObjects;
        private boolean indentArrays;
        private String copyright;

        private Builder() {
            this.indentObjects = false;
            this.indentArrays = false;
            this.copyright = null;
        }

        public Builder indentObjects() {
            this.indentObjects = true;
            return this;
        }

        public Builder indentArrays() {
            this.indentArrays = true;
            return this;
        }

        public Builder copyright(String copyright) {
            this.copyright = copyright;
            return this;
        }
        
        public Options build() {
            return new Options(this);
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="56f41492-6a7c-4bb4-a5cf-00956b750812" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
    out, 
    json, 
    Options.builder()
        .indentObjects()
        .indentArrays()
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .indentObjects()
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .indentArrays()
        .build()
);
writeJson(out, json, Options.standard());
writeJson(
    out, 
    json, 
    Options.builder()
        .indentObjects()
        .indentArrays()
        .copyright("(c) 2022")
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .indentObjects()
        .copyright("(c) 2022")
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .indentArrays()
        .copyright("(c) 2022")
        .build()
);
writeJson(
    out, 
    json, 
    Options.builder()
        .copyright("(c) 2022")
        .build()
);
```

</div>

</div>

------------------------------------------------------------------------

# Hypothetical 4.

It would be a lot more efficient if you started sending your JSON in a binary format like MessagePack. It has the same data model as JSON, so it should work out.

Also, when sending in that binary format there is a choice between "Little Endian" and "Big Endian".

Problem is, there really isn't a meaning to the indentation in a binary format or to endianness in a text one.

## Option 20. Make separate methods

For a split as fundamental as this, it might make sense to start to make an entirely separate API for the new JSON-like format.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="01444fba-2068-415a-972a-c024a62c1e4a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum Indent {
    OBJECTS,
    ARRAYS
}

...
        
enum Endianness {
    BIG_ENDIAN,
    LITTLE_ENDIAN
}
  
...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            EnumSet<Indent> indent,
            Optional<String> copyright
    ) {
        ...
    }
    
    public static void writeMessagePack(
            Appendable out,
            Json json,
            Endianness endianness,
            Optional<String> copyright
    ) {
        ...
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5661aa75-5fa7-485b-a1bc-d6ec1fa37b3d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
        out, 
        json,
        EnumSet.of(Indent.OBJECTS),
        Optional.of("(c) 2022")
);
writeMessagePack(
        out,
        json,
        Endianness.BIG_ENDIAN,
        Optional.of("(c) 2022")
);
```

</div>

</div>

## Option 21. Make an interface and use dispatch

You were surprised you didn't think of this first. Dynamic dispatch is some classic Java stylings.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b70466e8-f7b0-4a94-ba22-c3b5ae8ccc90" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public interface JsonWriter {
    void write(Appendable out, JSON json);
}

...

public final class TextJsonWriter implements JsonWriter {
    private final boolean indentObjects;
    private final boolean indentArrays;
    private final String copyright;
    
    public TextJsonWriter(
            boolean indentObjects, 
            boolean indentArrays,
            String copyright
    ) {}
    
    @Override
    public void write(Appendable out, JSON json) {
        ...
    }
}

...

enum Endianness {
    BIG_ENDIAN,
    LITTLE_ENDIAN
}

...

public final class BinaryJsonWriter implements JsonWriter {
    private final Endianness endianness;
    private final String copyright;

    public BinaryJsonWriter(
            Endianness endianness,
            String copyright
    ) {}
    
    @Override
    public void write(Appendable out, JSON json) {
        ...
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="84cf6286-7d4f-4cdf-ae5d-02fd357a31e1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
new BinaryJsonWriter(
        Endianness.BIG_ENDIAN,
        "(c) 2022"
).writeJson(out, json);

new TextJsonWriter(
        true,
        false,
        "(c) 2022"
).writeJson(out, json);
```

</div>

</div>

## Option 22. Take everything as an object and figure it out at runtime.

You need to choose whether you silently ignore bad combinations of objects and what behaviors get preference, but there is a simplicity to just throwing it all into a record or opaque object and figuring it out from there.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e7fbb88b-83e1-4b00-9bab-06d94e9cb64b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
enum Endianness {
    BIG_ENDIAN,
    LITTLE_ENDIAN
}

record Options(
        Boolean indentObjects, 
        Boolean indentArrays,
        String copyright,
        boolean useBinary,
        Endianness endianness
) {
    public static final Options INDENT_EVERYTHING =
            new Options(
                    true, 
                    true, 
                    null, 
                    false, 
                    null
            );
    public static final Options NO_INDENT =
            new Options(
                    false, 
                    false, 
                    null, 
                    false, 
                    null
            );
    public static final Options BINARY_LE = 
            new Options(
                    null,
                    null, 
                    null, 
                    true, 
                    Endianness.LITTLE_ENDIAN
            );
}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            Options options
    ) {
        if (options.useBinary() &&
                (options.indentArrays() != null 
                        || options.indentObjects() != null)) {
            // ignore or throw
            ...
        }
        else if (!options.useBinary() &&
                    options.endianness() != null) {
            ...
        }
        else {
            ...
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8b5b9411-e13d-4006-ab4f-b282a6226459" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
        out, 
        json, 
        Options.INDENT_EVERYTHING
);
writeJson(
        out,
        json,
        Options.BINARY_LE
);
writeJson(
        out,
        json,
        new Options(
            null,
            null, 
            null,
            true, 
            Endianness.BIG_ENDIAN
        )
);
```

</div>

</div>

## Option 23. Model valid choices in the type hierarchy.

With a bit of restructuring, you can actually make an `Options` object that will correctly handle having that disjoint set of options.

Maybe not what you would choose with 100 settings or more complicated legality restrictions, but for this case it all seems to work out.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c44d6c78-edce-4f6e-a67b-4a0bf2582e9f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
enum Endianness {
    BIG_ENDIAN,
    LITTLE_ENDIAN
}

sealed interface Options permits BinaryOptions, TextOptions {
    Optional<String> copyright();
}

record TextOptions(
        @Override Optional<String> copyright,
        boolean indentObjects,
        boolean indentArrays
) implements Options {}

record BinaryOptions(
        @Override Optional<String> copyright,
        Endianness endianness
) implements Options {}

...

public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            Options options
    ) {
        switch (options) {
            case TextOptions textOptions -> {
                ...
            }
            case BinaryOptions binaryOptions ->
                switch (binaryOptions.endianness()) {
                    case BIG_ENDIAN -> ...
                    case LITTLE_ENDIAN -> ...
                }
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="afea102a-9908-4127-9468-98365cfcba7e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
        out, 
        json, 
        new TextOptions(Optional.of("(c) 2022"), true, false)
);
writeJson(
        out,
        json,
        new BinaryOptions(Optional.empty(), Endianness.BIG_ENDIAN)
);
```

</div>

</div>

## Option 24. Give up on typing it, just pass a map

This was always an option. It works just as well here as it does in a dynamic language, it's just a tad more verbose and unsafe.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="243a6e43-aa71-4b1b-99c8-1cfc13c84e65" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public final class JsonWriter {
    private JsonWriter() {}

    public static void writeJson(
            Appendable out,
            Json json,
            Map<String, Object> options
    ) {
        var copyright = options.get("copyright");
        if (copyright == null) {
            ...
            var endianness = options.get("binary");
            ...
        }
        else if (copyright instanceof String copyrightString) {
            ...
        }
        else {
            throw new IllegalArgumentException(...);
        }
    }
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cf8e9d7f-6408-4bc6-aacf-3365e6fa496b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
writeJson(
        out, 
        json,
        Map.of(
            "indentObjects", true,
            "copyright", "(c) 2022"
        )
);
writeJson(
        out, 
        json,
        Map.of(
            "binary", true,
            "endianness", Endianness.BIG_ENDIAN
        )
);
```

</div>

</div>

------------------------------------------------------------------------

# Hypothetical 5.

You've taken the mouse to the movies. People don't need all the configuration options you've provided and don't like using the API that has them. They want a simpler API.

Maybe you should have gone with option 1.

References: <a href="https://mccue.dev/pages/2-8-22-options-for-options" class="external-link" data-card-appearance="inline" rel="nofollow">https://mccue.dev/pages/2-8-22-options-for-options</a>
