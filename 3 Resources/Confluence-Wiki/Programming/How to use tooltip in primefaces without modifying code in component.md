---
title: "How to use tooltip in primefaces without modifying code in component"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/PT/pages/25287339612/How+to+use+tooltip+in+primefaces+without+modifying+code+in+component
space: "PT"
topic: programming
relevance: 0.792
depth: 3
updated: 2017-04-19
attachments: 0
tags:
  - confluence
  - programming
  - space/pt
---

# How to use tooltip in primefaces without modifying code in component

> [!info] Imported from Confluence
> Space **PT** · updated 2017-04-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/PT/pages/25287339612/How+to+use+tooltip+in+primefaces+without+modifying+code+in+component)
> Relevance 0.792 · topic `programming`

1.  **Requirement**  
    We had a requirement to add some tooltips for buttons but without changing the code from the component that contains those buttons.  
      
2.  **Solution**

We have implemented a jquery plugin to fullfil that requirement  
  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9f573afd-4b16-469e-849e-13e6d697f670" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$.fn.customTooltip = function(options) {
        var defaults = {
                showEvent: 'mouseover.customTooltip',
                hideEvent: 'mouseout.customTooltip',
                tooltip: {
                    customTooltip: '',
                    styleClass: 'ui-tooltip ui-widget ui-widget-content ui-shadow ui-corner-all'
                }
        }

        var settings = $.extend(true, {}, defaults, options);

        return this.each(function(index, element) {
            var base = element,
                $base = $(element);

            base.init = function() {
                $('[customTooltip="'+settings.tooltip.customTooltip+'"]').addClass(settings.tooltip.styleClass);
            };

            base.bindTarget =  function() {
                $base.off(settings.showEvent + ' ' + settings.hideEvent).on(settings.showEvent, function(event) {
                    $('[customTooltip="'+settings.tooltip.customTooltip+'"]').show();

                    $('[customTooltip="'+settings.tooltip.customTooltip+'"]').position({
                        my: 'left top',
                        at: 'right bottom',
                        of: event.target,
                        collision: 'flipfit'
                    })

                }).on(settings.hideEvent, function() {
                    $('[customTooltip="'+settings.tooltip.customTooltip+'"]').hide();
                });
            };

            base.init();
            base.bindTarget();

        });
    }
```

</div>

</div>

- 

1.  1.   Adding a **div **tag to your page. Feel free to add cms content that you want to display in tooltip

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d8772ff9-53fb-49b3-8ec6-db159b054619" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        <h:panelGroup pt:customTooltip="tooltip" styleClass="" layout="block"> #{ ivy.cms.co('') }</h:panelGroup>
        ```

        </div>

        </div>

    2.  Register your element with tooltip plugin

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="63d530ae-a966-4be6-8c2a-a4ae3a0a74c5" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        $(function(){
            $('.element').customTooltip({
                tooltip: { customTooltip: 'tooltip' }
            });
        })
        ```

        </div>

        </div>

        Enjoy your coding <img src="https://jira.axonivy.com/confluence/s/en_GB/7103/9740d52e06037c926d0bef8c46735f0805791491/_/images/icons/emoticons/smile.png" title="(smile)" class="emoticon emoticon-smile" data-border="0" alt="(smile)" />

    **          Related Document :**

    - Different between  DOM Object vs Jquerry Object : <a href="http://howtodoinjava.com/scripting/jquery/javascript-dom-objects-vs-jquery-objects/" class="external-link" rel="nofollow">http://howtodoinjava.com/scripting/jquery/javascript-dom-objects-vs-jquery-objects/</a>

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="199a9edf-e0d5-4588-8dd0-348d828ec78b" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      var base = element,
      $base = $(element);
      ```

      </div>

      </div>

      ``` auto-cursor-target
      ```

    - mouseover.customTooltip

          event namespace https://api.jquery.com/event.namespace/

      

    - Merge Object : <a href="https://api.jquery.com/jquery.extend/" class="external-link" rel="nofollow">https://api.jquery.com/jquery.extend/</a>  
        

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7cc79773-7954-4925-84d1-9dea13dda529" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      var settings = $.extend(true, {}, defaults, options);
      ```

      </div>

      </div>

    - Set position base on mouse's position : <a href="https://api.jqueryui.com/position/" class="external-link" rel="nofollow">https://api.jqueryui.com/position/</a>.

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4ddb0375-a1ab-4778-8899-ee7b016bc205" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      $('[customTooltip="'+settings.tooltip.customTooltip+'"]').position({
                              my: 'left top',
                              at: 'right bottom',
                              of: event.target,
                              collision: 'flipfit'
                          })
      ```

      </div>

      </div>
