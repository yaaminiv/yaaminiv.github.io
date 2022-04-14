---
layout: post
comments: true
title: CEABiGR
tags: virginica mixOmics sPLS
---

## Trying to tune the sPLS

Based on my [initial sPLS](https://yaaminiv.github.io/CEABiGR/) results, I needed to separate male and female samples and run individual analyses to understand the relationship between pH, exon methylation, and expression.

I extracted the female data expression and methylation data and filtered it the same I did the larger dataset: removed exons with any missing values for any sample and lowly variable exons:

```
exonMethylationFem <- exonMethylationMod %>%
  filter(., grepl("F", rownames(.), fixed = TRUE)) %>%
  t(.) %>%
  as.data.frame(.) %>%
  rownames_to_column("exon") %>%
  drop_na(.) %>%
  rowwise() %>%
  mutate(range = max(c_across(where(is.numeric))) - min(c_across(where(is.numeric)))) %>%
  column_to_rownames("exon") %>%
  filter(., range > 10) %>%
  select(., !range) %>%
  t(.) %>%
  as.data.frame(.)
```

After filtering the data, I went through the process of tuning parameters described in the [`mixOmics` manual](https://mixomicsteam.github.io/Bookdown/pls.html#tuning:PLS). Before I could run an sPLS, I needed to ensure that the parameters I was using were appropriate for the data. The first parameter I tested was the number of components to include in the analysis. I ran a PLS with the female data, specifying `ncomp = 4` for four components.

```
exonResult.plsFem <- pls(exonMethylationFem, exonExpressionFem, ncomp = 4) #Run a PLS with a sufficient number of components
```

I then tried to test the significance of these components using `perf`, which supplies a k-fold cross-validation:

```
exonPerf.plsFem <- perf(exonResult.plsFem, validation = "Mfold", folds = 10,
                     progressBar = FALSE, nrepeat = 1000) #Use perf for repeated k-fold cross-validation to determine the number of components that should be included
```

Unfortunately, R Studio crashed! Around the time I started running the code, `raven` needed to be rebooted. I tried the code again and R Studio still crashed, so it wasn't just computer rebooting that caused the issue. I tried running `perf` again with `folds = 3` and `nrepeat = 100` to see if I could reduce processing power, but I still ended up crashing R Studio. I tried these things over a few weeks when I had time, so the saga was really prolonged.

At this point, I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1442) to get insight from Sam and Steven. Sam confirmed that `raven` did not have enough processing power to handle the `perf` command, and suggested running the code on the R Studio server on `mox` instead.

Before moving over to `mox` completely, I decided to try running the same commands using a subset of the data to troubleshoot the code. This way, I could let code run on `mox` without having to fix errors. Turns out using a subset didn't work, since `mixOmics` needs a substantial number of overlapping exons in both methylation and expression datasets! I kept running into errors, so I tried messing around with tuning X and Y. This also lead to R Studio crashing, even when using two components, two folds, and only ten repeats. Yikes.

Looks like I have to use `mox` no matter what! But first, I'll focus on updating the paper with foundational methods and results so when I return to this analysis it has a clear purpose.

### Going forward

1. Update foundation methods
2. Update foundation results
3. Revise with new expression data from Ariana
2. Tune sPLS parameters
3. Run sex-specific SPLS
4. Identify drivers

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://the-responsible-grad-student.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

{% endif %}

<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
