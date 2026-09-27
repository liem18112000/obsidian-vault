---
ai_hash: 269eabf07c7b7fd2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.906
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47235399737/Ionic+angular
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Ionic/angular
topic: programming
type: source
updated: 2022-12-15
---

# Ionic/angular

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-12-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47235399737/Ionic+angular)
> Relevance 0.906 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2" macro-id="5ca4728f6170d23f6ac906e896a02a65" macro-name="toc">

</div>

## I. Requirement:

- See more detail in <a href="https://update.angular.io/?v=10.0-15.0" class="external-link" rel="nofollow">Compare Angular v10 vs v15</a>

- These are main points that are impacted to our code

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Angular 10.x.x</strong></p></th>
<th><p><strong>Angular 15.x.x</strong></p>
<p>(2022-12-07)</p></th>
<th><p><strong>Docs:</strong></p></th>
</tr>
&#10;<tr>
<td><h4 id="Ionic/angular-nodejs">nodejs</h4></td>
<td><p><code>14.16.0</code></p></td>
<td><p><code>14.20.x, 16.13.x and 18.10.x or later</code></p></td>
<td></td>
</tr>
<tr>
<td><p>typescript</p></td>
<td><p><code>3.9.7</code></p></td>
<td><p><code>4.8 or later</code></p></td>
<td><p><a href="https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-8.html" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-8.html</a></p></td>
</tr>
<tr>
<td><p>tsconfig.json</p></td>
<td></td>
<td><p>remove <code>enableIvy</code></p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## II. Deprecated features

- See more detail in <a href="https://angular.io/guide/deprecations" class="external-link" rel="nofollow">Deprecated APIs and features</a>

- These are deprecated features that are impacted to our code:

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><h4 id="Ionic/angular-AREA">AREA</h4></th>
<th><h4 id="Ionic/angular-APIORFEATURE">API OR FEATURE</h4></th>
<th><h4 id="Ionic/angular-DEPRECATEDIN">DEPRECATED IN</h4></th>
<th><h4 id="Ionic/angular-MAYBEREMOVEDIN">MAY BE REMOVED IN</h4></th>
</tr>
&#10;<tr>
<td rowspan="2"><p><code>@angular/core</code></p></td>
<td><p><code>entryComponents</code></p></td>
<td><p>v9</p></td>
<td><p>v11</p></td>
</tr>
<tr>
<td><p><code>ComponentFactory</code></p>
<p><code>ComponentFactoryResolver</code></p></td>
<td><p>v13</p></td>
<td><p>v16</p></td>
</tr>
<tr>
<td><p><code>@angular/forms</code></p></td>
<td><p><code>FormBuilder.group</code> legacy options parameter</p></td>
<td><p>v11</p></td>
<td><p>v14</p></td>
</tr>
</tbody>
</table>

</div>

### 1. @angular/core

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><h4 id="Ionic/angular-API"><strong>API</strong></h4></th>
<th><h4 id="Ionic/angular-REPLACEMENT"><strong>REPLACEMENT</strong></h4></th>
<th><h4 id="Ionic/angular-DEPRECATIONANNOUNCED"><strong>DEPRECATION ANNOUNCED</strong></h4></th>
<th><h4 id="Ionic/angular-DETAILS"><strong>DETAILS</strong></h4></th>
</tr>
&#10;<tr>
<td><p><code>ReflectiveInjector</code></p></td>
<td><p><code>Injector.create()</code></p></td>
<td><p>v5</p></td>
<td><p>See <code>ReflectiveInjector</code></p></td>
</tr>
<tr>
<td><p><code>entryComponents</code></p></td>
<td><p>none</p></td>
<td><p>v9</p></td>
<td><p>See <code>entryComponents</code></p></td>
</tr>
<tr>
<td><p><code>async</code></p></td>
<td><p><code>waitForAsync</code></p></td>
<td><p>v11</p></td>
<td><p>The <code>async</code> function from <code>@angular/core/testing</code> has been renamed to <code>waitForAsync</code> in order to avoid confusion with the native JavaScript <code>async</code> syntax. The existing function is deprecated and can be removed in a future version.</p></td>
</tr>
<tr>
<td><p><code>ComponentFactory</code></p></td>
<td><p>Use non-factory based framework APIs.</p></td>
<td><p>v13</p></td>
<td><p>Since Ivy, Component factories are not required. Angular provides other APIs where Component classes can be used directly.</p></td>
</tr>
<tr>
<td><p><code>ComponentFactoryResolver</code></p></td>
<td><p>Use non-factory based framework APIs.</p></td>
<td><p>v13</p></td>
<td><p>Since Ivy, Component factories are not required, thus there is no need to resolve them.</p></td>
</tr>
<tr>
<td><p><code>providedIn</code> with NgModule</p></td>
<td><p>Prefer <code>'root'</code> providers, or use NgModule <code>providers</code> if scoping to an NgModule is necessary</p></td>
<td><p>v15</p></td>
<td><p>none</p></td>
</tr>
<tr>
<td><p><code>providedIn: 'any'</code></p></td>
<td><p>none</p></td>
<td><p>v15</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

### 2. @angular/forms

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><h4 id="Ionic/angular-API.1"><strong>API</strong></h4></th>
<th><h4 id="Ionic/angular-REPLACEMENT.1"><strong>REPLACEMENT</strong></h4></th>
<th><h4 id="Ionic/angular-DEPRECATIONANNOUNCED.1"><strong>DEPRECATION ANNOUNCED</strong></h4></th>
<th><h4 id="Ionic/angular-DETAILS.1"><strong>DETAILS</strong></h4></th>
</tr>
&#10;<tr>
<td><p><code>ngModel</code> with reactive forms</p></td>
<td><p><code>FormControlDirective</code></p></td>
<td><p>v6</p></td>
<td><p>none</p></td>
</tr>
<tr>
<td><p><code>FormBuilder.group</code> legacy options parameter</p></td>
<td><p><code>AbstractControlOptions</code> parameter value</p></td>
<td><p>v11</p></td>
<td><p>none</p></td>
</tr>
</tbody>
</table>

</div>

### 3. @angular/core/testing

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><h4 id="Ionic/angular-API.2">API</h4></th>
<th><h4 id="Ionic/angular-REPLACEMENT.2">REPLACEMENT</h4></th>
<th><h4 id="Ionic/angular-DEPRECATIONANNOUNCED.2">DEPRECATION ANNOUNCED</h4></th>
<th><h4 id="Ionic/angular-DETAILS.2">DETAILS</h4></th>
</tr>
&#10;<tr>
<td><p>TestBed.get</p></td>
<td><p>TestBed.inject</p></td>
<td><p>v9</p></td>
<td><p>Same behavior, but type safe.</p></td>
</tr>
<tr>
<td><p>async</p></td>
<td><p>waitForAsync</p></td>
<td><p>v10</p></td>
<td><p>Same behavior, but rename to avoid confusion.</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Upgrade Ivy - Known issues]]
- [[Non-Java modules work with cgroupv2]]
- [[Kotlin migration plan]]

%% ai-graph-end %%