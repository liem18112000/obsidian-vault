---
ai_hash: 876c0dc64352ded8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675776049/Token+concept
space: LUZCOMP
status: reference
tags:
- confluence
- architecture
- space/luzcomp
title: Token concept
topic: architecture
type: source
updated: 2016-09-04
---

# Token concept

> [!info] Imported from Confluence
> Space **LUZCOMP** · updated 2016-09-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675776049/Token+concept)
> Relevance 0.711 · topic `architecture`

We will take the following page as reference: <a href="http://connect2id.com/products/nimbus-jose-jwt/examples" class="external-link" rel="nofollow">http://connect2id.com/products/nimbus-jose-jwt/examples</a> and its library (under Apache 2.0 licence) for JWT producing, encoding, and decoding.

<div hasbody="true" macro-id="82f5f350-ab88-4913-9ab9-1beda7871e4c" macro-name="warning">

<span class="aui-icon aui-icon-small aui-iconfont-error confluence-information-macro-icon"> </span>

<div>

My very first tests show a potential performance overhead depending on the type of security level chosen for signing the JWT.... We have to test and compare the different solutions...

</div>

</div>

 

## Token is signed using a secret (HMAC protection)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e07c2cab-069b-4471-b97f-a1c3a75a92cc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package io.axonivy.sec.jwt;
import static org.junit.Assert.assertTrue;
import org.junit.Rule;
import org.junit.Test;
import org.junit.rules.ExpectedException;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.SignatureException;
public class JWTWriterTest {
    
    private final static String SUBJECT = "ec";
    private final static String SUPER_SECRET = "my secret:D";
    
    @Rule
    public ExpectedException thrown = ExpectedException.none();
    
    @Test
    public void decodeToken_succesfull_with_shared_secret() {
        //We create a token that is HS512 signed with the given secret
        String token = produceTokenUsingSecretForSigning(SUPER_SECRET);
        
        // For being able to decode the token information we need to know (share) this secret
        String decodedSubject = Jwts.parser().setSigningKey(SUPER_SECRET).parseClaimsJws(token).getBody().getSubject();
        
        assertTrue(decodedSubject.equals(SUBJECT));
    }
    
    @Test
    public void decodeToken_throws_SignatureException_if_secret_used_for_decoding_not_the_same_as_for_encoding() {
        //We create a token that is HS512 signed with the given secret
        String token = produceTokenUsingSecretForSigning(SUPER_SECRET);
        
        // using an invalid secret throws a SignatureException
        thrown.expect(SignatureException.class);
        Jwts.parser().setSigningKey("dfhadh").parseClaimsJws(token).getBody().getSubject();
    }
    
    @Test
    public void decodeToken_throws_IAE_if_nosecret_used_for_decoding_not_the_same_as_for_encoding() {
        //We create a token that is HS512 signed with the given secret
        String token = produceTokenUsingSecretForSigning(SUPER_SECRET);
        
        // using no secret for decoding a jwt made with signature throws an IllegalArgumentException
        thrown.expect(IllegalArgumentException.class);
        Jwts.parser().parseClaimsJws(token).getBody().getSubject();
    }
    private String produceTokenUsingSecretForSigning(String secret) {
        return JWTWriter.withSecret(secret).
                withAlgorithm(SignatureAlgorithm.HS512).
                addClaim("sub", SUBJECT).
                addClaim("email", "ecomba@comba.fr").
                doToken();
    }
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Vault overview]]
- [[Two-grade JWTs solve the multi-tenant bootstrap basic token discovers tenants]]
- [[Token JWT Security]]
- [[Truncating a JWT breaks signature verification and surfaces as 500 not 401]]
- [[Authorization]]

%% ai-graph-end %%