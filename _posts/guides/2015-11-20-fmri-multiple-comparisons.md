---
layout: article
title: "fMRI and multiple comparisons correction"
categories: guides
excerpt: "How to correct correctly."
ads: false
share: false
toc: true
image:
  feature: decor.tower.jpg
  teaser: cover.multiple.png
---

Thanks to an [infamous dead salmon](http://pages.vassar.edu/abigailbaird/files/2014/06/bennett_salmon.pdf), the fMRI community in particular has been under a lot of pressure to manage the multitude of independent tests done in an average experiment with extreme care. For those in fields that don't typically have to think about this, multiple comparison correction deals with the fact that every (frequentist) statistic is associated with some risk that the result found is totally by chance. This is normally very acceptable. Most scientists are willing to accept that there is a 1% chance their results aren't actually real. The problem comes in when you run 10,000 tests, because we know that in this case, we'll get 100 of those 1% false-positives. This is called a Type I error.

We can reduce the number of Type I errors by making our tests stricter, but this often has the effect of increasing the false negative rate, or Type II error. Each of these kinds of errors have a very domain-specific effect. In cancer screening, the effect of a Type I error would likely lead to an anxious few weeks while a confirmation test is performed. A Type II error could lead to a preventable death. In fMRI research, a Type I error leads to a publication that is later discredited, while a Type II error means the research will [never see the light of day](http://apps.who.int/rhl/education/MR000006_butlerpa_com/en/). This can be really damaging to the field in the long term, because it puts a lot of downward pressure on the output.

Which is the crux of this gem of a paper: [Type I and Type II error concerns in fMRI research: re-balancing the scale](http://www.ncbi.nlm.nih.gov/pubmed/20035017). The authors essentially ask that scientists err more on the side of Type I errors than is common in the field, and rely on [replication samples](https://en.wikipedia.org/wiki/Test_set) to demonstrate how general the results are.

This brings us to another problem: in fMRI, voxels aren't really independent in the first place, it comes off the scanner with a certain level of smoothness, and as a matter of course, people typically spatially smooth the data further. So typical threshold procedures, even liberal ones, are going to be conservative if they assume independent tests. One way to get around this problem is to look at what a totally noisy dataset with a similar level of smoothness would produce, dead salmon-style. We can do this with AFNI's `3dClustSim`.

Note on `3dClustSim`: use a build of AFNI more recent than mid 2015. According to [Cluster failure: Why fMRI inferences for spatial extent have inflated false-positive rates](http://www.pnas.org/content/early/2016/06/27/1602413113), a 15 year old bug was found and fixed in `3dClustSim` that increased significance values by a good amount (p values were too low). This bug was fixed May 2015. Aside from being alarming, this reveals a significant problem with the modern scientific process: the **bug was found in early 2015, and the paper was published July 2016**. Furthermore reports of this bug are buried in a discussion point in the last page of the paper. Only the most attentive reader would find it.

This guide assumes you have been using [`epitome`](https://github.com/josephdviviano/epitome) to pre-process your data, but any data will do.

First, estimate the smoothness of the epitome output (data to be analyzed) using:

{% highlight bash %}

3dFWHMx -detrend func_volsmooth.vrmd-compcorr.01.nii.gz

{% endhighlight %}

Which outputs:

{% highlight bash %}

10.0868  10.0895  9.28078

{% endhighlight %}

This tells us our smoothness is approximately 10 mm FWHM in the x,y planes, and 9.2 in the z plane.

We'll do our analysis within the MNI-space mask to help us with our stats. The mask also tells `3dClustSim` what our voxel dimensions are (so make sure they are the same). `3dClustSim` is going to produce 10000 noise datasets, smooth them, and then detect clusters of high amplitude. It will tell us what the chance on finding a contiguous high-amplitude cluster in a dataset of pure noise at various thresholds and cluster sizes, given the voxel dimensions. I'm setting seed to 0, because this randomizes the seed with each run. I'm also going to use the default p thresholds because they are sensible. NN 3 means we'll consider voxels part of a cluster if their faces, edges, or corners touch.

{% highlight bash %}

3dClustSim -mask anat_EPI_mask_MNI-lin.nii.gz -fwhmxyz 10 10 9.2 -iter 10000 -seed 0 -NN 3

{% endhighlight %}

This returns a table of required cluster sizes at various p-thresholds, alpha levels, or cluster sizes. This is really handy (especially when used in conjunction with AFNI's excellent viewer) for exploring the data to make sure you haven't left too much out, or too much in, when you're writing up your results.

{% highlight bash %}

# CLUSTER SIZE THRESHOLD(pthr,alpha) in Voxels
# -NN 3  | alpha = Prob(Cluster >= given size)
#  pthr  | .10000 .05000 .02000 .01000
# ------ | ------ ------ ------ ------
 0.020000   628.5  719.0  851.0  953.0
 0.010000   360.4  415.5  489.7  547.0
 0.005000   235.0  270.8  315.8  353.0
 0.002000   142.9  166.2  197.4  220.0
 0.001000   100.5  118.8  143.3  160.7
 0.000500    71.6   85.9  106.3  122.8
 0.000200    46.2   57.3   73.6   85.0
 0.000100    32.2   42.1   55.5   65.6

{% endhighlight %}

Finally: consider combining this method with a [nonparametric statistical test](http://www.viviano.ca/writing/randomize/) for best results. Since fMRI data is autocorrelated, parametric statistics are likely to (falsely) report larger clusters of activation than nonparametric ones.
