<!-- README.md is generated from README.Rmd. Please edit that file -->

# [rprojroot](https://r-lib.github.io/rprojroot)

<!-- badges: start -->

[![rcc](https://github.com/r-lib/rprojroot/workflows/rcc/badge.svg)](https://github.com/r-lib/rprojroot/actions) [![codecov.io](https://codecov.io/github/r-lib/rprojroot/coverage.svg?branch=master)](https://codecov.io/github/r-lib/rprojroot?branch=master) [![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/rprojroot)](https://cran.r-project.org/package=rprojroot)

<!-- badges: end -->

This package helps accessing files relative to a *project root* to [stop the working directory insanity](https://gist.github.com/jennybc/362f52446fe1ebc4c49f). It is a low-level helper package for the [here](https://here.r-lib.org/) package.

<pre class='chroma'>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://github.com/r-lib/rprojroot'>rprojroot</a></span><span class='o'>)</span></pre>

## Example

The rprojroot package works best when you have a “project”: all related files contained in a subdirectory that can be categorized using a strict criterion. Let’s create a package for demonstration.

<pre class='chroma'>
<span class='nv'>dir</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/tempfile.html'>tempfile</a></span><span class='o'>(</span><span class='o'>)</span>
<span class='nv'>pkg</span> <span class='o'>&lt;-</span> <span class='nf'>usethis</span><span class='nf'>::</span><span class='nf'><a href='https://usethis.r-lib.org/reference/create_package.html'>create_package</a></span><span class='o'>(</span><span class='nv'>dir</span><span class='o'>)</span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✓</span><span> Creating </span><span style='color: #0000BB;'>'/tmp/RtmpG04Wy1/file2d4962962cd9/'</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✓</span><span> Setting active project to </span><span style='color: #0000BB;'>'/tmp/RtmpG04Wy1/file2d4962962cd9'</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✓</span><span> Creating </span><span style='color: #0000BB;'>'R/'</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✓</span><span> Writing </span><span style='color: #0000BB;'>'DESCRIPTION'</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Package</span><span>: file2d4962962cd9</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Title</span><span>: What the Package Does (One Line, Title Case)</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Version</span><span>: 0.0.0.9000</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Date</span><span>: 2020-11-08</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Authors@R</span><span> (parsed):</span></span>
<span class='c'>#&gt;     * Kirill Müller &lt;krlmlr+r@mailbox.org&gt; [aut, cre] (&lt;https://orcid.org/0000-0002-1416-3412&gt;)</span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Description</span><span>: What the package does (one paragraph).</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>License</span><span>: GPL-3</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>URL</span><span>: https://github.com/krlmlr/rprojroot,</span></span>
<span class='c'>#&gt;     https://krlmlr.github.io/rprojroot</span>
<span class='c'>#&gt; <span style='color: #0000BB;'>BugReports</span><span>: https://github.com/krlmlr/rprojroot/issues</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Encoding</span><span>: UTF-8</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>LazyData</span><span>: true</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>Roxygen</span><span>: list(markdown = TRUE)</span></span>
<span class='c'>#&gt; <span style='color: #0000BB;'>RoxygenNote</span><span>: 7.1.1.9000</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✓</span><span> Writing </span><span style='color: #0000BB;'>'NAMESPACE'</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✓</span><span> Setting active project to </span><span style='color: #0000BB;'>'&lt;no active project&gt;'</span></span></pre>

R packages satisfy the `is_r_package` criterion. A criterion is an object that contains a `find_file()` function. With `pkg` as working directory, the function works like [`file.path()`](https://rdrr.io/r/base/file.path.html), rooted at the working directory:

<pre class='chroma'>
<span class='nf'><a href='https://rdrr.io/r/base/getwd.html'>setwd</a></span><span class='o'>(</span><span class='nv'>pkg</span><span class='o'>)</span>
<span class='nv'>is_r_package</span>
<span class='c'>#&gt; Root criterion: contains a file `DESCRIPTION` with contents matching `^Package: `</span>
<span class='nv'>is_r_package</span><span class='o'>$</span><span class='nf'>find_file</span><span class='o'>(</span><span class='o'>)</span>
<span class='c'>#&gt; [1] "/tmp/RtmpG04Wy1/file2d4962962cd9"</span>
<span class='nv'>is_r_package</span><span class='o'>$</span><span class='nf'>find_file</span><span class='o'>(</span><span class='s'>"tests"</span>, <span class='s'>"testthat"</span><span class='o'>)</span>
<span class='c'>#&gt; [1] "/tmp/RtmpG04Wy1/file2d4962962cd9/tests/testthat"</span></pre>

This works identically when starting from a subdirectory:

<pre class='chroma'>
<span class='nf'><a href='https://rdrr.io/r/base/getwd.html'>setwd</a></span><span class='o'>(</span><span class='nf'><a href='https://rdrr.io/r/base/file.path.html'>file.path</a></span><span class='o'>(</span><span class='nv'>pkg</span>, <span class='s'>"R"</span><span class='o'>)</span><span class='o'>)</span>
<span class='nv'>is_r_package</span><span class='o'>$</span><span class='nf'>find_file</span><span class='o'>(</span><span class='o'>)</span>
<span class='c'>#&gt; [1] "/tmp/RtmpG04Wy1/file2d4962962cd9"</span>
<span class='nv'>is_r_package</span><span class='o'>$</span><span class='nf'>find_file</span><span class='o'>(</span><span class='s'>"tests"</span>, <span class='s'>"testthat"</span><span class='o'>)</span>
<span class='c'>#&gt; [1] "/tmp/RtmpG04Wy1/file2d4962962cd9/tests/testthat"</span></pre>

As long as you are sure that your working directory is somewhere inside your project, you can retrieve the project root.

## Installation and further reading

Install the package from CRAN:

<pre class='chroma'>
<span class='nf'>install.package</span><span class='o'>(</span><span class='s'>"rprojroot"</span><span class='o'>)</span></pre>

See the [documentation](https://r-lib.github.io/rprojroot/articles/rprojroot.html) for more detail.

------------------------------------------------------------------------

## Code of Conduct

Please note that the rprojroot project is released with a [Contributor Code of Conduct](https://rprojroot.r-lib.org/CODE_OF_CONDUCT.html). By contributing to this project, you agree to abide by its terms.
