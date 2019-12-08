cola Report for Consensus Partitioning
==================

**Date**: 2019-12-05 02:03:52 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 15448    54
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:hclust](#SD-hclust)     |          6| 1.000|           1.000|       1.000|** |5          |
|[SD:pam](#SD-pam)           |          6| 1.000|           0.945|       0.978|** |5          |
|[CV:skmeans](#CV-skmeans)   |          2| 1.000|           0.998|       0.999|** |           |
|[MAD:hclust](#MAD-hclust)   |          6| 1.000|           0.951|       0.979|** |5          |
|[MAD:pam](#MAD-pam)         |          6| 1.000|           0.959|       0.980|** |5          |
|[ATC:hclust](#ATC-hclust)   |          5| 1.000|           0.993|       0.993|** |2,4        |
|[ATC:pam](#ATC-pam)         |          6| 1.000|           0.957|       0.976|** |2,4        |
|[ATC:mclust](#ATC-mclust)   |          6| 1.000|           0.998|       0.999|** |2,5        |
|[CV:NMF](#CV-NMF)           |          6| 0.968|           0.880|       0.948|** |2          |
|[ATC:NMF](#ATC-NMF)         |          2| 0.961|           0.984|       0.991|** |           |
|[CV:pam](#CV-pam)           |          6| 0.945|           0.962|       0.955|*  |2,5        |
|[SD:mclust](#SD-mclust)     |          6| 0.942|           0.888|       0.941|*  |           |
|[SD:skmeans](#SD-skmeans)   |          5| 0.940|           0.955|       0.963|*  |           |
|[CV:hclust](#CV-hclust)     |          6| 0.934|           0.938|       0.945|*  |5          |
|[MAD:skmeans](#MAD-skmeans) |          6| 0.932|           0.935|       0.949|*  |5          |
|[SD:NMF](#SD-NMF)           |          6| 0.929|           0.880|       0.929|*  |           |
|[CV:mclust](#CV-mclust)     |          6| 0.914|           0.945|       0.943|*  |3,5        |
|[ATC:skmeans](#ATC-skmeans) |          5| 0.910|           0.946|       0.911|*  |2,3        |
|[MAD:NMF](#MAD-NMF)         |          6| 0.901|           0.795|       0.874|*  |           |
|[MAD:mclust](#MAD-mclust)   |          5| 0.871|           0.809|       0.891|   |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 0.792|           0.844|       0.933|   |           |
|[MAD:kmeans](#MAD-kmeans)   |          6| 0.757|           0.805|       0.762|   |           |
|[SD:kmeans](#SD-kmeans)     |          5| 0.540|           0.809|       0.781|   |           |
|[CV:kmeans](#CV-kmeans)     |          2| 0.196|           0.816|       0.847|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 0.287           0.679       0.812          0.450 0.516   0.516
#&gt; CV:NMF      2 0.939           0.932       0.963          0.493 0.497   0.497
#&gt; MAD:NMF     2 0.624           0.906       0.929          0.493 0.491   0.491
#&gt; ATC:NMF     2 0.961           0.984       0.991          0.507 0.491   0.491
#&gt; SD:skmeans  2 0.500           0.788       0.857          0.501 0.491   0.491
#&gt; CV:skmeans  2 1.000           0.998       0.999          0.509 0.491   0.491
#&gt; MAD:skmeans 2 0.500           0.888       0.913          0.505 0.491   0.491
#&gt; ATC:skmeans 2 1.000           1.000       1.000          0.509 0.491   0.491
#&gt; SD:mclust   2 0.302           0.672       0.749          0.424 0.560   0.560
#&gt; CV:mclust   2 0.432           0.920       0.913          0.376 0.648   0.648
#&gt; MAD:mclust  2 0.305           0.735       0.764          0.385 0.648   0.648
#&gt; ATC:mclust  2 1.000           0.958       0.967          0.415 0.591   0.591
#&gt; SD:kmeans   2 0.151           0.494       0.709          0.429 0.560   0.560
#&gt; CV:kmeans   2 0.196           0.816       0.847          0.466 0.491   0.491
#&gt; MAD:kmeans  2 0.184           0.568       0.723          0.456 0.560   0.560
#&gt; ATC:kmeans  2 0.792           0.844       0.933          0.479 0.547   0.547
#&gt; SD:pam      2 0.497           0.878       0.890          0.336 0.717   0.717
#&gt; CV:pam      2 1.000           0.973       0.983          0.365 0.648   0.648
#&gt; MAD:pam     2 0.397           0.627       0.831          0.497 0.497   0.497
#&gt; ATC:pam     2 1.000           0.921       0.974          0.431 0.575   0.575
#&gt; SD:hclust   2 0.497           0.759       0.829          0.456 0.516   0.516
#&gt; CV:hclust   2 0.730           0.890       0.947          0.466 0.547   0.547
#&gt; MAD:hclust  2 0.516           0.739       0.849          0.472 0.516   0.516
#&gt; ATC:hclust  2 1.000           1.000       1.000          0.509 0.491   0.491
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.732           0.873       0.941          0.352 0.761   0.580
#&gt; CV:NMF      3 0.690           0.772       0.890          0.273 0.820   0.657
#&gt; MAD:NMF     3 0.880           0.893       0.954          0.274 0.822   0.651
#&gt; ATC:NMF     3 0.823           0.915       0.915          0.212 0.893   0.782
#&gt; SD:skmeans  3 0.866           0.959       0.969          0.339 0.683   0.441
#&gt; CV:skmeans  3 0.807           0.916       0.936          0.303 0.792   0.598
#&gt; MAD:skmeans 3 0.866           0.901       0.945          0.326 0.683   0.441
#&gt; ATC:skmeans 3 1.000           0.991       0.990          0.198 0.899   0.795
#&gt; SD:mclust   3 0.415           0.593       0.741          0.414 0.790   0.626
#&gt; CV:mclust   3 0.929           0.896       0.926          0.751 0.629   0.449
#&gt; MAD:mclust  3 0.291           0.572       0.687          0.607 0.748   0.612
#&gt; ATC:mclust  3 0.676           0.905       0.911          0.384 0.623   0.464
#&gt; SD:kmeans   3 0.245           0.428       0.591          0.385 0.790   0.626
#&gt; CV:kmeans   3 0.338           0.414       0.612          0.338 0.798   0.621
#&gt; MAD:kmeans  3 0.308           0.643       0.668          0.352 0.765   0.581
#&gt; ATC:kmeans  3 0.452           0.766       0.794          0.291 0.795   0.637
#&gt; SD:pam      3 0.444           0.757       0.803          0.825 0.646   0.507
#&gt; CV:pam      3 0.644           0.937       0.936          0.660 0.748   0.612
#&gt; MAD:pam     3 0.479           0.651       0.776          0.206 0.623   0.433
#&gt; ATC:pam     3 0.730           0.853       0.914          0.386 0.711   0.538
#&gt; SD:hclust   3 0.686           0.901       0.893          0.235 0.849   0.707
#&gt; CV:hclust   3 0.500           0.784       0.769          0.304 0.799   0.632
#&gt; MAD:hclust  3 0.723           0.882       0.913          0.242 0.849   0.707
#&gt; ATC:hclust  3 0.893           0.951       0.956          0.211 0.885   0.765
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.885           0.883       0.939         0.2396 0.831   0.577
#&gt; CV:NMF      4 0.856           0.916       0.954         0.1979 0.807   0.524
#&gt; MAD:NMF     4 0.860           0.900       0.942         0.1974 0.856   0.611
#&gt; ATC:NMF     4 0.606           0.686       0.821         0.1656 0.829   0.581
#&gt; SD:skmeans  4 0.764           0.852       0.847         0.1130 0.925   0.768
#&gt; CV:skmeans  4 0.762           0.865       0.892         0.1289 0.891   0.685
#&gt; MAD:skmeans 4 0.855           0.941       0.948         0.1208 0.925   0.768
#&gt; ATC:skmeans 4 0.807           0.966       0.940         0.2005 0.866   0.657
#&gt; SD:mclust   4 0.627           0.711       0.807         0.1221 0.672   0.330
#&gt; CV:mclust   4 0.814           0.890       0.922         0.1225 0.799   0.484
#&gt; MAD:mclust  4 0.438           0.634       0.797         0.1662 0.728   0.413
#&gt; ATC:mclust  4 0.831           0.925       0.933         0.2426 0.849   0.657
#&gt; SD:kmeans   4 0.402           0.593       0.660         0.1788 0.684   0.329
#&gt; CV:kmeans   4 0.403           0.594       0.661         0.1406 0.795   0.502
#&gt; MAD:kmeans  4 0.457           0.669       0.714         0.1504 0.937   0.807
#&gt; ATC:kmeans  4 0.584           0.680       0.683         0.1393 0.881   0.682
#&gt; SD:pam      4 0.786           0.893       0.894         0.1628 0.877   0.688
#&gt; CV:pam      4 0.818           0.940       0.950         0.2165 0.868   0.667
#&gt; MAD:pam     4 0.738           0.844       0.905         0.2276 0.772   0.516
#&gt; ATC:pam     4 1.000           0.977       0.988         0.1173 0.929   0.820
#&gt; SD:hclust   4 0.711           0.602       0.734         0.2371 0.711   0.410
#&gt; CV:hclust   4 0.617           0.763       0.763         0.1671 0.943   0.836
#&gt; MAD:hclust  4 0.774           0.923       0.956         0.1992 0.937   0.828
#&gt; ATC:hclust  4 1.000           0.991       0.990         0.0756 0.962   0.900
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.838           0.853       0.904         0.0661 0.834   0.454
#&gt; CV:NMF      5 0.891           0.913       0.951         0.0650 0.857   0.519
#&gt; MAD:NMF     5 0.845           0.794       0.897         0.0721 0.855   0.499
#&gt; ATC:NMF     5 0.753           0.876       0.869         0.0899 0.888   0.602
#&gt; SD:skmeans  5 0.940           0.955       0.963         0.0812 0.894   0.610
#&gt; CV:skmeans  5 0.857           0.915       0.925         0.0790 0.894   0.610
#&gt; MAD:skmeans 5 0.908           0.940       0.955         0.0758 0.894   0.610
#&gt; ATC:skmeans 5 0.910           0.946       0.911         0.0747 0.943   0.779
#&gt; SD:mclust   5 0.868           0.912       0.934         0.1792 0.899   0.667
#&gt; CV:mclust   5 0.918           0.959       0.966         0.0804 0.950   0.800
#&gt; MAD:mclust  5 0.871           0.809       0.891         0.1131 0.885   0.592
#&gt; ATC:mclust  5 1.000           0.993       0.996         0.1364 0.899   0.652
#&gt; SD:kmeans   5 0.540           0.809       0.781         0.0948 0.881   0.601
#&gt; CV:kmeans   5 0.556           0.642       0.687         0.0805 0.936   0.750
#&gt; MAD:kmeans  5 0.622           0.770       0.778         0.0887 0.894   0.627
#&gt; ATC:kmeans  5 0.631           0.352       0.682         0.0863 0.860   0.569
#&gt; SD:pam      5 0.937           0.970       0.984         0.1198 0.899   0.667
#&gt; CV:pam      5 1.000           1.000       1.000         0.1007 0.925   0.714
#&gt; MAD:pam     5 1.000           0.956       0.975         0.0974 0.885   0.592
#&gt; ATC:pam     5 0.866           0.857       0.908         0.1329 0.933   0.802
#&gt; SD:hclust   5 1.000           1.000       1.000         0.1466 0.874   0.615
#&gt; CV:hclust   5 0.937           0.935       0.947         0.1356 0.899   0.652
#&gt; MAD:hclust  5 1.000           0.951       0.979         0.1430 0.899   0.667
#&gt; ATC:hclust  5 1.000           0.993       0.993         0.0991 0.933   0.802
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.929           0.880       0.929         0.0257 0.920   0.648
#&gt; CV:NMF      6 0.968           0.880       0.948         0.0446 0.923   0.652
#&gt; MAD:NMF     6 0.901           0.795       0.874         0.0259 0.971   0.849
#&gt; ATC:NMF     6 0.819           0.737       0.783         0.0518 0.915   0.643
#&gt; SD:skmeans  6 0.896           0.901       0.928         0.0343 0.978   0.881
#&gt; CV:skmeans  6 0.869           0.818       0.824         0.0330 0.978   0.881
#&gt; MAD:skmeans 6 0.932           0.935       0.949         0.0354 0.978   0.881
#&gt; ATC:skmeans 6 0.862           0.800       0.827         0.0428 0.981   0.906
#&gt; SD:mclust   6 0.942           0.888       0.941         0.0473 0.969   0.847
#&gt; CV:mclust   6 0.914           0.945       0.943         0.0333 0.978   0.889
#&gt; MAD:mclust  6 0.844           0.798       0.866         0.0351 0.978   0.881
#&gt; ATC:mclust  6 1.000           0.998       0.999         0.0282 0.978   0.881
#&gt; SD:kmeans   6 0.759           0.766       0.779         0.0469 1.000   1.000
#&gt; CV:kmeans   6 0.698           0.615       0.698         0.0467 0.962   0.826
#&gt; MAD:kmeans  6 0.757           0.805       0.762         0.0522 0.978   0.889
#&gt; ATC:kmeans  6 0.685           0.797       0.766         0.0458 0.864   0.516
#&gt; SD:pam      6 1.000           0.945       0.978         0.0326 0.978   0.889
#&gt; CV:pam      6 0.945           0.962       0.955         0.0270 0.978   0.881
#&gt; MAD:pam     6 1.000           0.959       0.980         0.0257 0.948   0.748
#&gt; ATC:pam     6 1.000           0.957       0.976         0.0927 0.899   0.629
#&gt; SD:hclust   6 1.000           1.000       1.000         0.0280 0.978   0.889
#&gt; CV:hclust   6 0.934           0.938       0.945         0.0331 0.978   0.881
#&gt; MAD:hclust  6 1.000           0.951       0.979         0.0278 0.978   0.889
#&gt; ATC:hclust  6 0.873           0.958       0.860         0.0817 0.899   0.629
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>



 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.497           0.759       0.829          0.456 0.516   0.516
#> 3 3 0.686           0.901       0.893          0.235 0.849   0.707
#> 4 4 0.711           0.602       0.734          0.237 0.711   0.410
#> 5 5 1.000           1.000       1.000          0.147 0.874   0.615
#> 6 6 1.000           1.000       1.000          0.028 0.978   0.889
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     1   0.000      0.835 1.000 0.000
#&gt; SRR1576834     1   0.000      0.835 1.000 0.000
#&gt; SRR1576835     1   0.000      0.835 1.000 0.000
#&gt; SRR1576836     1   0.000      0.835 1.000 0.000
#&gt; SRR1576837     1   0.000      0.835 1.000 0.000
#&gt; SRR1576838     1   0.000      0.835 1.000 0.000
#&gt; SRR1576839     1   0.000      0.835 1.000 0.000
#&gt; SRR1576840     1   0.000      0.835 1.000 0.000
#&gt; SRR1576841     2   0.961      0.818 0.384 0.616
#&gt; SRR1576842     2   0.961      0.818 0.384 0.616
#&gt; SRR1576843     2   0.961      0.818 0.384 0.616
#&gt; SRR1576844     2   0.961      0.818 0.384 0.616
#&gt; SRR1576845     2   0.961      0.818 0.384 0.616
#&gt; SRR1576846     2   0.961      0.818 0.384 0.616
#&gt; SRR1576847     2   0.961      0.818 0.384 0.616
#&gt; SRR1576848     2   0.961      0.818 0.384 0.616
#&gt; SRR1576849     2   0.961      0.818 0.384 0.616
#&gt; SRR1576850     2   0.961      0.818 0.384 0.616
#&gt; SRR1576851     2   0.961      0.818 0.384 0.616
#&gt; SRR1576852     2   0.961      0.818 0.384 0.616
#&gt; SRR1576853     1   0.000      0.835 1.000 0.000
#&gt; SRR1576854     1   0.000      0.835 1.000 0.000
#&gt; SRR1576855     1   0.000      0.835 1.000 0.000
#&gt; SRR1576856     1   0.000      0.835 1.000 0.000
#&gt; SRR1576857     2   0.000      0.600 0.000 1.000
#&gt; SRR1576858     2   0.000      0.600 0.000 1.000
#&gt; SRR1576859     2   0.000      0.600 0.000 1.000
#&gt; SRR1576860     2   0.000      0.600 0.000 1.000
#&gt; SRR1576861     2   0.000      0.600 0.000 1.000
#&gt; SRR1576862     2   0.000      0.600 0.000 1.000
#&gt; SRR1576863     1   0.000      0.835 1.000 0.000
#&gt; SRR1576864     1   0.000      0.835 1.000 0.000
#&gt; SRR1576865     1   0.000      0.835 1.000 0.000
#&gt; SRR1576866     1   0.000      0.835 1.000 0.000
#&gt; SRR1576867     1   0.963      0.565 0.612 0.388
#&gt; SRR1576868     1   0.963      0.565 0.612 0.388
#&gt; SRR1576869     1   0.963      0.565 0.612 0.388
#&gt; SRR1576870     1   0.963      0.565 0.612 0.388
#&gt; SRR1576871     1   0.963      0.565 0.612 0.388
#&gt; SRR1576872     1   0.963      0.565 0.612 0.388
#&gt; SRR1576873     1   0.000      0.835 1.000 0.000
#&gt; SRR1576874     1   0.000      0.835 1.000 0.000
#&gt; SRR1576875     1   0.000      0.835 1.000 0.000
#&gt; SRR1576876     1   0.000      0.835 1.000 0.000
#&gt; SRR1576877     1   0.963      0.565 0.612 0.388
#&gt; SRR1576878     1   0.963      0.565 0.612 0.388
#&gt; SRR1576879     1   0.963      0.565 0.612 0.388
#&gt; SRR1576880     2   0.961      0.818 0.384 0.616
#&gt; SRR1576881     2   0.961      0.818 0.384 0.616
#&gt; SRR1576882     2   0.961      0.818 0.384 0.616
#&gt; SRR1576883     1   0.000      0.835 1.000 0.000
#&gt; SRR1576884     1   0.000      0.835 1.000 0.000
#&gt; SRR1576885     1   0.000      0.835 1.000 0.000
#&gt; SRR1576886     1   0.000      0.835 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1576833     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576834     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576835     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576836     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576837     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576838     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576839     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576840     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576841     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576842     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576843     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576844     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576845     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576846     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576847     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576848     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576849     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576850     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576851     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576852     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576853     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576854     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576855     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576856     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576857     3   0.000      0.575  0 0.000 1.000
#&gt; SRR1576858     3   0.000      0.575  0 0.000 1.000
#&gt; SRR1576859     3   0.000      0.575  0 0.000 1.000
#&gt; SRR1576860     3   0.000      0.575  0 0.000 1.000
#&gt; SRR1576861     3   0.000      0.575  0 0.000 1.000
#&gt; SRR1576862     3   0.000      0.575  0 0.000 1.000
#&gt; SRR1576863     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576864     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576865     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576866     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576867     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576868     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576869     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576870     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576871     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576872     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576873     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576874     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576875     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576876     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576877     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576878     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576879     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1576880     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576881     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576882     3   0.606      0.813  0 0.384 0.616
#&gt; SRR1576883     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576884     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576885     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1576886     2   0.000      1.000  0 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2  p3    p4
#&gt; SRR1576833     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576834     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576835     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576836     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576837     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576838     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576839     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576840     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576841     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576842     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576843     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576844     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576845     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576846     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576847     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576848     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576849     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576850     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576851     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576852     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576853     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576854     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576855     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576856     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576857     3   0.000      1.000  0 0.000 1.0 0.000
#&gt; SRR1576858     3   0.000      1.000  0 0.000 1.0 0.000
#&gt; SRR1576859     3   0.000      1.000  0 0.000 1.0 0.000
#&gt; SRR1576860     3   0.000      1.000  0 0.000 1.0 0.000
#&gt; SRR1576861     3   0.000      1.000  0 0.000 1.0 0.000
#&gt; SRR1576862     3   0.000      1.000  0 0.000 1.0 0.000
#&gt; SRR1576863     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576864     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576865     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576866     4   0.493      0.036  0 0.432 0.0 0.568
#&gt; SRR1576867     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576868     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576869     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576870     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576871     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576872     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576873     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576874     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576875     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576876     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576877     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576878     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576879     1   0.000      1.000  1 0.000 0.0 0.000
#&gt; SRR1576880     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576881     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576882     4   0.759      0.338  0 0.368 0.2 0.432
#&gt; SRR1576883     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576884     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576885     2   0.000      1.000  0 1.000 0.0 0.000
#&gt; SRR1576886     2   0.000      1.000  0 1.000 0.0 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5
#&gt; SRR1576833     2       0          1  0  1  0  0  0
#&gt; SRR1576834     2       0          1  0  1  0  0  0
#&gt; SRR1576835     2       0          1  0  1  0  0  0
#&gt; SRR1576836     2       0          1  0  1  0  0  0
#&gt; SRR1576837     5       0          1  0  0  0  0  1
#&gt; SRR1576838     5       0          1  0  0  0  0  1
#&gt; SRR1576839     5       0          1  0  0  0  0  1
#&gt; SRR1576840     5       0          1  0  0  0  0  1
#&gt; SRR1576841     4       0          1  0  0  0  1  0
#&gt; SRR1576842     4       0          1  0  0  0  1  0
#&gt; SRR1576843     4       0          1  0  0  0  1  0
#&gt; SRR1576844     4       0          1  0  0  0  1  0
#&gt; SRR1576845     4       0          1  0  0  0  1  0
#&gt; SRR1576846     4       0          1  0  0  0  1  0
#&gt; SRR1576847     4       0          1  0  0  0  1  0
#&gt; SRR1576848     4       0          1  0  0  0  1  0
#&gt; SRR1576849     4       0          1  0  0  0  1  0
#&gt; SRR1576850     4       0          1  0  0  0  1  0
#&gt; SRR1576851     4       0          1  0  0  0  1  0
#&gt; SRR1576852     4       0          1  0  0  0  1  0
#&gt; SRR1576853     5       0          1  0  0  0  0  1
#&gt; SRR1576854     5       0          1  0  0  0  0  1
#&gt; SRR1576855     5       0          1  0  0  0  0  1
#&gt; SRR1576856     5       0          1  0  0  0  0  1
#&gt; SRR1576857     3       0          1  0  0  1  0  0
#&gt; SRR1576858     3       0          1  0  0  1  0  0
#&gt; SRR1576859     3       0          1  0  0  1  0  0
#&gt; SRR1576860     3       0          1  0  0  1  0  0
#&gt; SRR1576861     3       0          1  0  0  1  0  0
#&gt; SRR1576862     3       0          1  0  0  1  0  0
#&gt; SRR1576863     5       0          1  0  0  0  0  1
#&gt; SRR1576864     5       0          1  0  0  0  0  1
#&gt; SRR1576865     5       0          1  0  0  0  0  1
#&gt; SRR1576866     5       0          1  0  0  0  0  1
#&gt; SRR1576867     1       0          1  1  0  0  0  0
#&gt; SRR1576868     1       0          1  1  0  0  0  0
#&gt; SRR1576869     1       0          1  1  0  0  0  0
#&gt; SRR1576870     1       0          1  1  0  0  0  0
#&gt; SRR1576871     1       0          1  1  0  0  0  0
#&gt; SRR1576872     1       0          1  1  0  0  0  0
#&gt; SRR1576873     2       0          1  0  1  0  0  0
#&gt; SRR1576874     2       0          1  0  1  0  0  0
#&gt; SRR1576875     2       0          1  0  1  0  0  0
#&gt; SRR1576876     2       0          1  0  1  0  0  0
#&gt; SRR1576877     1       0          1  1  0  0  0  0
#&gt; SRR1576878     1       0          1  1  0  0  0  0
#&gt; SRR1576879     1       0          1  1  0  0  0  0
#&gt; SRR1576880     4       0          1  0  0  0  1  0
#&gt; SRR1576881     4       0          1  0  0  0  1  0
#&gt; SRR1576882     4       0          1  0  0  0  1  0
#&gt; SRR1576883     2       0          1  0  1  0  0  0
#&gt; SRR1576884     2       0          1  0  1  0  0  0
#&gt; SRR1576885     2       0          1  0  1  0  0  0
#&gt; SRR1576886     2       0          1  0  1  0  0  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1576833     2       0          1  0  1  0  0  0  0
#&gt; SRR1576834     2       0          1  0  1  0  0  0  0
#&gt; SRR1576835     2       0          1  0  1  0  0  0  0
#&gt; SRR1576836     2       0          1  0  1  0  0  0  0
#&gt; SRR1576837     5       0          1  0  0  0  0  1  0
#&gt; SRR1576838     5       0          1  0  0  0  0  1  0
#&gt; SRR1576839     5       0          1  0  0  0  0  1  0
#&gt; SRR1576840     5       0          1  0  0  0  0  1  0
#&gt; SRR1576841     4       0          1  0  0  0  1  0  0
#&gt; SRR1576842     4       0          1  0  0  0  1  0  0
#&gt; SRR1576843     4       0          1  0  0  0  1  0  0
#&gt; SRR1576844     4       0          1  0  0  0  1  0  0
#&gt; SRR1576845     4       0          1  0  0  0  1  0  0
#&gt; SRR1576846     4       0          1  0  0  0  1  0  0
#&gt; SRR1576847     4       0          1  0  0  0  1  0  0
#&gt; SRR1576848     4       0          1  0  0  0  1  0  0
#&gt; SRR1576849     4       0          1  0  0  0  1  0  0
#&gt; SRR1576850     4       0          1  0  0  0  1  0  0
#&gt; SRR1576851     4       0          1  0  0  0  1  0  0
#&gt; SRR1576852     4       0          1  0  0  0  1  0  0
#&gt; SRR1576853     5       0          1  0  0  0  0  1  0
#&gt; SRR1576854     5       0          1  0  0  0  0  1  0
#&gt; SRR1576855     5       0          1  0  0  0  0  1  0
#&gt; SRR1576856     5       0          1  0  0  0  0  1  0
#&gt; SRR1576857     3       0          1  0  0  1  0  0  0
#&gt; SRR1576858     3       0          1  0  0  1  0  0  0
#&gt; SRR1576859     3       0          1  0  0  1  0  0  0
#&gt; SRR1576860     3       0          1  0  0  1  0  0  0
#&gt; SRR1576861     3       0          1  0  0  1  0  0  0
#&gt; SRR1576862     3       0          1  0  0  1  0  0  0
#&gt; SRR1576863     6       0          1  0  0  0  0  0  1
#&gt; SRR1576864     6       0          1  0  0  0  0  0  1
#&gt; SRR1576865     6       0          1  0  0  0  0  0  1
#&gt; SRR1576866     6       0          1  0  0  0  0  0  1
#&gt; SRR1576867     1       0          1  1  0  0  0  0  0
#&gt; SRR1576868     1       0          1  1  0  0  0  0  0
#&gt; SRR1576869     1       0          1  1  0  0  0  0  0
#&gt; SRR1576870     1       0          1  1  0  0  0  0  0
#&gt; SRR1576871     1       0          1  1  0  0  0  0  0
#&gt; SRR1576872     1       0          1  1  0  0  0  0  0
#&gt; SRR1576873     2       0          1  0  1  0  0  0  0
#&gt; SRR1576874     2       0          1  0  1  0  0  0  0
#&gt; SRR1576875     2       0          1  0  1  0  0  0  0
#&gt; SRR1576876     2       0          1  0  1  0  0  0  0
#&gt; SRR1576877     1       0          1  1  0  0  0  0  0
#&gt; SRR1576878     1       0          1  1  0  0  0  0  0
#&gt; SRR1576879     1       0          1  1  0  0  0  0  0
#&gt; SRR1576880     4       0          1  0  0  0  1  0  0
#&gt; SRR1576881     4       0          1  0  0  0  1  0  0
#&gt; SRR1576882     4       0          1  0  0  0  1  0  0
#&gt; SRR1576883     2       0          1  0  1  0  0  0  0
#&gt; SRR1576884     2       0          1  0  1  0  0  0  0
#&gt; SRR1576885     2       0          1  0  1  0  0  0  0
#&gt; SRR1576886     2       0          1  0  1  0  0  0  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.151           0.494       0.709         0.4292 0.560   0.560
#> 3 3 0.245           0.428       0.591         0.3850 0.790   0.626
#> 4 4 0.402           0.593       0.660         0.1788 0.684   0.329
#> 5 5 0.540           0.809       0.781         0.0948 0.881   0.601
#> 6 6 0.759           0.766       0.779         0.0469 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576834     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576835     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576836     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576837     1  0.9635     0.5251 0.612 0.388
#&gt; SRR1576838     1  0.9635     0.5251 0.612 0.388
#&gt; SRR1576839     1  0.9850     0.4305 0.572 0.428
#&gt; SRR1576840     1  0.9850     0.4305 0.572 0.428
#&gt; SRR1576841     2  0.8499     0.3267 0.276 0.724
#&gt; SRR1576842     2  0.8499     0.3267 0.276 0.724
#&gt; SRR1576843     2  0.8499     0.3267 0.276 0.724
#&gt; SRR1576844     2  0.0672     0.6044 0.008 0.992
#&gt; SRR1576845     2  0.0672     0.6044 0.008 0.992
#&gt; SRR1576846     2  0.0672     0.6044 0.008 0.992
#&gt; SRR1576847     2  0.1414     0.6054 0.020 0.980
#&gt; SRR1576848     2  0.1414     0.6054 0.020 0.980
#&gt; SRR1576849     2  0.1414     0.6054 0.020 0.980
#&gt; SRR1576850     2  0.2236     0.6009 0.036 0.964
#&gt; SRR1576851     2  0.2236     0.6009 0.036 0.964
#&gt; SRR1576852     2  0.2236     0.6009 0.036 0.964
#&gt; SRR1576853     1  0.9323     0.5151 0.652 0.348
#&gt; SRR1576854     1  0.9323     0.5151 0.652 0.348
#&gt; SRR1576855     1  0.9129     0.5449 0.672 0.328
#&gt; SRR1576856     1  0.9129     0.5449 0.672 0.328
#&gt; SRR1576857     2  0.9977     0.0201 0.472 0.528
#&gt; SRR1576858     2  0.9977     0.0201 0.472 0.528
#&gt; SRR1576859     2  0.9977     0.0201 0.472 0.528
#&gt; SRR1576860     2  0.9977     0.0201 0.472 0.528
#&gt; SRR1576861     2  0.9977     0.0201 0.472 0.528
#&gt; SRR1576862     2  0.9977     0.0201 0.472 0.528
#&gt; SRR1576863     2  0.9580     0.3772 0.380 0.620
#&gt; SRR1576864     2  0.9580     0.3772 0.380 0.620
#&gt; SRR1576865     2  0.9580     0.3772 0.380 0.620
#&gt; SRR1576866     2  0.9580     0.3772 0.380 0.620
#&gt; SRR1576867     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576868     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576869     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576870     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576871     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576872     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576873     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576874     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576875     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576876     2  0.8207     0.5222 0.256 0.744
#&gt; SRR1576877     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576878     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576879     1  0.5178     0.7302 0.884 0.116
#&gt; SRR1576880     2  0.1414     0.6054 0.020 0.980
#&gt; SRR1576881     2  0.1414     0.6054 0.020 0.980
#&gt; SRR1576882     2  0.1414     0.6054 0.020 0.980
#&gt; SRR1576883     2  0.8499     0.5045 0.276 0.724
#&gt; SRR1576884     2  0.8499     0.5045 0.276 0.724
#&gt; SRR1576885     2  0.8499     0.5045 0.276 0.724
#&gt; SRR1576886     2  0.8499     0.5045 0.276 0.724
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576834     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576835     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576836     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576837     1   0.931     0.4361 0.488 0.336 0.176
#&gt; SRR1576838     1   0.931     0.4361 0.488 0.336 0.176
#&gt; SRR1576839     1   0.937     0.3847 0.464 0.360 0.176
#&gt; SRR1576840     1   0.937     0.3847 0.464 0.360 0.176
#&gt; SRR1576841     3   0.722     0.5216 0.052 0.296 0.652
#&gt; SRR1576842     3   0.722     0.5216 0.052 0.296 0.652
#&gt; SRR1576843     3   0.722     0.5216 0.052 0.296 0.652
#&gt; SRR1576844     2   0.624    -0.0141 0.000 0.560 0.440
#&gt; SRR1576845     2   0.624    -0.0141 0.000 0.560 0.440
#&gt; SRR1576846     2   0.624    -0.0141 0.000 0.560 0.440
#&gt; SRR1576847     2   0.629    -0.0701 0.000 0.536 0.464
#&gt; SRR1576848     2   0.629    -0.0701 0.000 0.536 0.464
#&gt; SRR1576849     2   0.629    -0.0701 0.000 0.536 0.464
#&gt; SRR1576850     3   0.668     0.0768 0.008 0.488 0.504
#&gt; SRR1576851     3   0.668     0.0768 0.008 0.488 0.504
#&gt; SRR1576852     3   0.668     0.0768 0.008 0.488 0.504
#&gt; SRR1576853     1   0.969     0.4576 0.452 0.308 0.240
#&gt; SRR1576854     1   0.969     0.4576 0.452 0.308 0.240
#&gt; SRR1576855     1   0.966     0.4617 0.456 0.308 0.236
#&gt; SRR1576856     1   0.966     0.4617 0.456 0.308 0.236
#&gt; SRR1576857     3   0.729     0.6210 0.212 0.092 0.696
#&gt; SRR1576858     3   0.729     0.6210 0.212 0.092 0.696
#&gt; SRR1576859     3   0.729     0.6210 0.212 0.092 0.696
#&gt; SRR1576860     3   0.737     0.6210 0.220 0.092 0.688
#&gt; SRR1576861     3   0.737     0.6210 0.220 0.092 0.688
#&gt; SRR1576862     3   0.737     0.6210 0.220 0.092 0.688
#&gt; SRR1576863     2   0.825     0.3683 0.164 0.636 0.200
#&gt; SRR1576864     2   0.825     0.3683 0.164 0.636 0.200
#&gt; SRR1576865     2   0.825     0.3683 0.164 0.636 0.200
#&gt; SRR1576866     2   0.825     0.3683 0.164 0.636 0.200
#&gt; SRR1576867     1   0.117     0.6667 0.976 0.016 0.008
#&gt; SRR1576868     1   0.117     0.6667 0.976 0.016 0.008
#&gt; SRR1576869     1   0.117     0.6667 0.976 0.016 0.008
#&gt; SRR1576870     1   0.117     0.6667 0.976 0.016 0.008
#&gt; SRR1576871     1   0.117     0.6667 0.976 0.016 0.008
#&gt; SRR1576872     1   0.117     0.6667 0.976 0.016 0.008
#&gt; SRR1576873     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576874     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576875     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576876     2   0.494     0.5815 0.148 0.824 0.028
#&gt; SRR1576877     1   0.177     0.6646 0.960 0.016 0.024
#&gt; SRR1576878     1   0.177     0.6646 0.960 0.016 0.024
#&gt; SRR1576879     1   0.177     0.6646 0.960 0.016 0.024
#&gt; SRR1576880     2   0.625    -0.0210 0.000 0.556 0.444
#&gt; SRR1576881     2   0.625    -0.0210 0.000 0.556 0.444
#&gt; SRR1576882     2   0.625    -0.0210 0.000 0.556 0.444
#&gt; SRR1576883     2   0.448     0.5761 0.136 0.844 0.020
#&gt; SRR1576884     2   0.448     0.5761 0.136 0.844 0.020
#&gt; SRR1576885     2   0.448     0.5761 0.136 0.844 0.020
#&gt; SRR1576886     2   0.448     0.5761 0.136 0.844 0.020
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2   0.174      0.680 0.004 0.940 0.056 0.000
#&gt; SRR1576834     2   0.174      0.680 0.004 0.940 0.056 0.000
#&gt; SRR1576835     2   0.174      0.680 0.004 0.940 0.056 0.000
#&gt; SRR1576836     2   0.174      0.680 0.004 0.940 0.056 0.000
#&gt; SRR1576837     2   0.923     -0.345 0.324 0.332 0.076 0.268
#&gt; SRR1576838     2   0.923     -0.345 0.324 0.332 0.076 0.268
#&gt; SRR1576839     2   0.927     -0.334 0.312 0.340 0.080 0.268
#&gt; SRR1576840     2   0.927     -0.334 0.312 0.340 0.080 0.268
#&gt; SRR1576841     3   0.620      0.604 0.044 0.072 0.720 0.164
#&gt; SRR1576842     3   0.620      0.604 0.044 0.072 0.720 0.164
#&gt; SRR1576843     3   0.620      0.604 0.044 0.072 0.720 0.164
#&gt; SRR1576844     3   0.395      0.652 0.000 0.216 0.780 0.004
#&gt; SRR1576845     3   0.395      0.652 0.000 0.216 0.780 0.004
#&gt; SRR1576846     3   0.395      0.652 0.000 0.216 0.780 0.004
#&gt; SRR1576847     3   0.440      0.662 0.008 0.180 0.792 0.020
#&gt; SRR1576848     3   0.440      0.662 0.008 0.180 0.792 0.020
#&gt; SRR1576849     3   0.440      0.662 0.008 0.180 0.792 0.020
#&gt; SRR1576850     3   0.564      0.617 0.008 0.140 0.740 0.112
#&gt; SRR1576851     3   0.564      0.617 0.008 0.140 0.740 0.112
#&gt; SRR1576852     3   0.564      0.617 0.008 0.140 0.740 0.112
#&gt; SRR1576853     4   0.826      0.663 0.276 0.240 0.024 0.460
#&gt; SRR1576854     4   0.826      0.663 0.276 0.240 0.024 0.460
#&gt; SRR1576855     4   0.818      0.660 0.284 0.236 0.020 0.460
#&gt; SRR1576856     4   0.818      0.660 0.284 0.236 0.020 0.460
#&gt; SRR1576857     3   0.837      0.385 0.156 0.044 0.416 0.384
#&gt; SRR1576858     3   0.837      0.385 0.156 0.044 0.416 0.384
#&gt; SRR1576859     3   0.837      0.385 0.156 0.044 0.416 0.384
#&gt; SRR1576860     3   0.834      0.385 0.152 0.044 0.408 0.396
#&gt; SRR1576861     3   0.834      0.385 0.152 0.044 0.408 0.396
#&gt; SRR1576862     3   0.834      0.385 0.152 0.044 0.408 0.396
#&gt; SRR1576863     4   0.820      0.634 0.084 0.356 0.084 0.476
#&gt; SRR1576864     4   0.820      0.634 0.084 0.356 0.084 0.476
#&gt; SRR1576865     4   0.820      0.634 0.084 0.356 0.084 0.476
#&gt; SRR1576866     4   0.820      0.634 0.084 0.356 0.084 0.476
#&gt; SRR1576867     1   0.182      0.957 0.936 0.060 0.004 0.000
#&gt; SRR1576868     1   0.182      0.957 0.936 0.060 0.004 0.000
#&gt; SRR1576869     1   0.182      0.957 0.936 0.060 0.004 0.000
#&gt; SRR1576870     1   0.182      0.957 0.936 0.060 0.004 0.000
#&gt; SRR1576871     1   0.182      0.957 0.936 0.060 0.004 0.000
#&gt; SRR1576872     1   0.182      0.957 0.936 0.060 0.004 0.000
#&gt; SRR1576873     2   0.182      0.680 0.004 0.936 0.060 0.000
#&gt; SRR1576874     2   0.182      0.680 0.004 0.936 0.060 0.000
#&gt; SRR1576875     2   0.182      0.680 0.004 0.936 0.060 0.000
#&gt; SRR1576876     2   0.182      0.680 0.004 0.936 0.060 0.000
#&gt; SRR1576877     1   0.461      0.913 0.828 0.056 0.036 0.080
#&gt; SRR1576878     1   0.461      0.913 0.828 0.056 0.036 0.080
#&gt; SRR1576879     1   0.461      0.913 0.828 0.056 0.036 0.080
#&gt; SRR1576880     3   0.361      0.658 0.000 0.200 0.800 0.000
#&gt; SRR1576881     3   0.361      0.658 0.000 0.200 0.800 0.000
#&gt; SRR1576882     3   0.361      0.658 0.000 0.200 0.800 0.000
#&gt; SRR1576883     2   0.322      0.590 0.008 0.888 0.036 0.068
#&gt; SRR1576884     2   0.322      0.590 0.008 0.888 0.036 0.068
#&gt; SRR1576885     2   0.322      0.590 0.008 0.888 0.036 0.068
#&gt; SRR1576886     2   0.322      0.590 0.008 0.888 0.036 0.068
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2   0.136      0.900 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576834     2   0.136      0.900 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576835     2   0.136      0.900 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576836     2   0.136      0.900 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576837     5   0.873      0.678 0.152 0.248 0.128 0.048 0.424
#&gt; SRR1576838     5   0.873      0.678 0.152 0.248 0.128 0.048 0.424
#&gt; SRR1576839     5   0.872      0.676 0.148 0.252 0.128 0.048 0.424
#&gt; SRR1576840     5   0.872      0.676 0.148 0.252 0.128 0.048 0.424
#&gt; SRR1576841     4   0.588      0.501 0.008 0.028 0.256 0.644 0.064
#&gt; SRR1576842     4   0.588      0.501 0.008 0.028 0.256 0.644 0.064
#&gt; SRR1576843     4   0.588      0.501 0.008 0.028 0.256 0.644 0.064
#&gt; SRR1576844     4   0.303      0.829 0.000 0.128 0.016 0.852 0.004
#&gt; SRR1576845     4   0.303      0.829 0.000 0.128 0.016 0.852 0.004
#&gt; SRR1576846     4   0.303      0.829 0.000 0.128 0.016 0.852 0.004
#&gt; SRR1576847     4   0.410      0.824 0.008 0.096 0.044 0.824 0.028
#&gt; SRR1576848     4   0.410      0.824 0.008 0.096 0.044 0.824 0.028
#&gt; SRR1576849     4   0.410      0.824 0.008 0.096 0.044 0.824 0.028
#&gt; SRR1576850     4   0.454      0.764 0.008 0.044 0.048 0.800 0.100
#&gt; SRR1576851     4   0.454      0.764 0.008 0.044 0.048 0.800 0.100
#&gt; SRR1576852     4   0.454      0.764 0.008 0.044 0.048 0.800 0.100
#&gt; SRR1576853     5   0.734      0.730 0.148 0.128 0.080 0.040 0.604
#&gt; SRR1576854     5   0.734      0.730 0.148 0.128 0.080 0.040 0.604
#&gt; SRR1576855     5   0.734      0.730 0.148 0.128 0.080 0.040 0.604
#&gt; SRR1576856     5   0.734      0.730 0.148 0.128 0.080 0.040 0.604
#&gt; SRR1576857     3   0.456      0.989 0.048 0.020 0.792 0.124 0.016
#&gt; SRR1576858     3   0.456      0.989 0.048 0.020 0.792 0.124 0.016
#&gt; SRR1576859     3   0.456      0.989 0.048 0.020 0.792 0.124 0.016
#&gt; SRR1576860     3   0.502      0.989 0.052 0.020 0.768 0.128 0.032
#&gt; SRR1576861     3   0.502      0.989 0.052 0.020 0.768 0.128 0.032
#&gt; SRR1576862     3   0.502      0.989 0.052 0.020 0.768 0.128 0.032
#&gt; SRR1576863     5   0.651      0.569 0.016 0.152 0.080 0.092 0.660
#&gt; SRR1576864     5   0.651      0.569 0.016 0.152 0.080 0.092 0.660
#&gt; SRR1576865     5   0.646      0.569 0.016 0.152 0.076 0.092 0.664
#&gt; SRR1576866     5   0.646      0.569 0.016 0.152 0.076 0.092 0.664
#&gt; SRR1576867     1   0.096      0.930 0.972 0.016 0.008 0.000 0.004
#&gt; SRR1576868     1   0.096      0.930 0.972 0.016 0.008 0.000 0.004
#&gt; SRR1576869     1   0.096      0.930 0.972 0.016 0.008 0.000 0.004
#&gt; SRR1576870     1   0.144      0.930 0.956 0.016 0.020 0.004 0.004
#&gt; SRR1576871     1   0.144      0.930 0.956 0.016 0.020 0.004 0.004
#&gt; SRR1576872     1   0.144      0.930 0.956 0.016 0.020 0.004 0.004
#&gt; SRR1576873     2   0.136      0.898 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576874     2   0.136      0.898 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576875     2   0.136      0.898 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576876     2   0.136      0.898 0.000 0.956 0.012 0.028 0.004
#&gt; SRR1576877     1   0.480      0.864 0.792 0.028 0.044 0.036 0.100
#&gt; SRR1576878     1   0.480      0.864 0.792 0.028 0.044 0.036 0.100
#&gt; SRR1576879     1   0.483      0.864 0.792 0.032 0.044 0.036 0.096
#&gt; SRR1576880     4   0.247      0.833 0.000 0.104 0.012 0.884 0.000
#&gt; SRR1576881     4   0.247      0.833 0.000 0.104 0.012 0.884 0.000
#&gt; SRR1576882     4   0.247      0.833 0.000 0.104 0.012 0.884 0.000
#&gt; SRR1576883     2   0.435      0.808 0.008 0.808 0.028 0.052 0.104
#&gt; SRR1576884     2   0.435      0.808 0.008 0.808 0.028 0.052 0.104
#&gt; SRR1576885     2   0.435      0.808 0.008 0.808 0.028 0.052 0.104
#&gt; SRR1576886     2   0.435      0.808 0.008 0.808 0.028 0.052 0.104
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1576833     2  0.1065      0.863 0.000 0.964 0.008 0.020 0.000 NA
#&gt; SRR1576834     2  0.1065      0.863 0.000 0.964 0.008 0.020 0.000 NA
#&gt; SRR1576835     2  0.1065      0.863 0.000 0.964 0.008 0.020 0.000 NA
#&gt; SRR1576836     2  0.1065      0.863 0.000 0.964 0.008 0.020 0.000 NA
#&gt; SRR1576837     5  0.8961      0.635 0.148 0.220 0.076 0.024 0.272 NA
#&gt; SRR1576838     5  0.8961      0.635 0.148 0.220 0.076 0.024 0.272 NA
#&gt; SRR1576839     5  0.8933      0.635 0.148 0.220 0.072 0.024 0.272 NA
#&gt; SRR1576840     5  0.8933      0.635 0.148 0.220 0.072 0.024 0.272 NA
#&gt; SRR1576841     4  0.6566      0.467 0.004 0.004 0.196 0.476 0.028 NA
#&gt; SRR1576842     4  0.6566      0.467 0.004 0.004 0.196 0.476 0.028 NA
#&gt; SRR1576843     4  0.6566      0.467 0.004 0.004 0.196 0.476 0.028 NA
#&gt; SRR1576844     4  0.4233      0.771 0.000 0.040 0.004 0.724 0.008 NA
#&gt; SRR1576845     4  0.4233      0.771 0.000 0.040 0.004 0.724 0.008 NA
#&gt; SRR1576846     4  0.4233      0.771 0.000 0.040 0.004 0.724 0.008 NA
#&gt; SRR1576847     4  0.2088      0.748 0.000 0.036 0.004 0.920 0.024 NA
#&gt; SRR1576848     4  0.2088      0.748 0.000 0.036 0.004 0.920 0.024 NA
#&gt; SRR1576849     4  0.2088      0.748 0.000 0.036 0.004 0.920 0.024 NA
#&gt; SRR1576850     4  0.2425      0.717 0.008 0.012 0.000 0.880 0.100 NA
#&gt; SRR1576851     4  0.2425      0.717 0.008 0.012 0.000 0.880 0.100 NA
#&gt; SRR1576852     4  0.2425      0.717 0.008 0.012 0.000 0.880 0.100 NA
#&gt; SRR1576853     5  0.7617      0.692 0.144 0.096 0.052 0.008 0.500 NA
#&gt; SRR1576854     5  0.7617      0.692 0.144 0.096 0.052 0.008 0.500 NA
#&gt; SRR1576855     5  0.7617      0.692 0.144 0.096 0.052 0.008 0.500 NA
#&gt; SRR1576856     5  0.7617      0.692 0.144 0.096 0.052 0.008 0.500 NA
#&gt; SRR1576857     3  0.4649      0.953 0.028 0.004 0.772 0.092 0.024 NA
#&gt; SRR1576858     3  0.4649      0.953 0.028 0.004 0.772 0.092 0.024 NA
#&gt; SRR1576859     3  0.4649      0.953 0.028 0.004 0.772 0.092 0.024 NA
#&gt; SRR1576860     3  0.2618      0.953 0.024 0.004 0.876 0.092 0.004 NA
#&gt; SRR1576861     3  0.2618      0.953 0.024 0.004 0.876 0.092 0.004 NA
#&gt; SRR1576862     3  0.2618      0.953 0.024 0.004 0.876 0.092 0.004 NA
#&gt; SRR1576863     5  0.2425      0.533 0.012 0.100 0.000 0.008 0.880 NA
#&gt; SRR1576864     5  0.2425      0.533 0.012 0.100 0.000 0.008 0.880 NA
#&gt; SRR1576865     5  0.2425      0.533 0.012 0.100 0.000 0.008 0.880 NA
#&gt; SRR1576866     5  0.2425      0.533 0.012 0.100 0.000 0.008 0.880 NA
#&gt; SRR1576867     1  0.0547      0.904 0.980 0.020 0.000 0.000 0.000 NA
#&gt; SRR1576868     1  0.0603      0.904 0.980 0.016 0.004 0.000 0.000 NA
#&gt; SRR1576869     1  0.0547      0.904 0.980 0.020 0.000 0.000 0.000 NA
#&gt; SRR1576870     1  0.1419      0.904 0.952 0.016 0.016 0.004 0.012 NA
#&gt; SRR1576871     1  0.1419      0.904 0.952 0.016 0.016 0.004 0.012 NA
#&gt; SRR1576872     1  0.1419      0.904 0.952 0.016 0.016 0.004 0.012 NA
#&gt; SRR1576873     2  0.1837      0.860 0.000 0.932 0.012 0.020 0.004 NA
#&gt; SRR1576874     2  0.1837      0.860 0.000 0.932 0.012 0.020 0.004 NA
#&gt; SRR1576875     2  0.1837      0.860 0.000 0.932 0.012 0.020 0.004 NA
#&gt; SRR1576876     2  0.1837      0.860 0.000 0.932 0.012 0.020 0.004 NA
#&gt; SRR1576877     1  0.4317      0.822 0.732 0.012 0.032 0.000 0.012 NA
#&gt; SRR1576878     1  0.4317      0.822 0.732 0.012 0.032 0.000 0.012 NA
#&gt; SRR1576879     1  0.4528      0.822 0.732 0.012 0.036 0.004 0.016 NA
#&gt; SRR1576880     4  0.3836      0.778 0.000 0.040 0.000 0.764 0.008 NA
#&gt; SRR1576881     4  0.3836      0.778 0.000 0.040 0.000 0.764 0.008 NA
#&gt; SRR1576882     4  0.3836      0.778 0.000 0.040 0.000 0.764 0.008 NA
#&gt; SRR1576883     2  0.5446      0.744 0.004 0.704 0.040 0.020 0.120 NA
#&gt; SRR1576884     2  0.5446      0.744 0.004 0.704 0.040 0.020 0.120 NA
#&gt; SRR1576885     2  0.5362      0.744 0.004 0.704 0.028 0.020 0.120 NA
#&gt; SRR1576886     2  0.5362      0.744 0.004 0.704 0.028 0.020 0.120 NA
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.500           0.788       0.857         0.5009 0.491   0.491
#> 3 3 0.866           0.959       0.969         0.3388 0.683   0.441
#> 4 4 0.764           0.852       0.847         0.1130 0.925   0.768
#> 5 5 0.940           0.955       0.963         0.0812 0.894   0.610
#> 6 6 0.896           0.901       0.928         0.0343 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.000      0.811 0.000 1.000
#&gt; SRR1576834     2   0.000      0.811 0.000 1.000
#&gt; SRR1576835     2   0.000      0.811 0.000 1.000
#&gt; SRR1576836     2   0.000      0.811 0.000 1.000
#&gt; SRR1576837     1   0.876      0.848 0.704 0.296
#&gt; SRR1576838     1   0.876      0.848 0.704 0.296
#&gt; SRR1576839     1   0.876      0.848 0.704 0.296
#&gt; SRR1576840     1   0.876      0.848 0.704 0.296
#&gt; SRR1576841     1   0.482      0.601 0.896 0.104
#&gt; SRR1576842     1   0.482      0.601 0.896 0.104
#&gt; SRR1576843     1   0.482      0.601 0.896 0.104
#&gt; SRR1576844     2   0.876      0.763 0.296 0.704
#&gt; SRR1576845     2   0.876      0.763 0.296 0.704
#&gt; SRR1576846     2   0.876      0.763 0.296 0.704
#&gt; SRR1576847     2   0.876      0.763 0.296 0.704
#&gt; SRR1576848     2   0.876      0.763 0.296 0.704
#&gt; SRR1576849     2   0.876      0.763 0.296 0.704
#&gt; SRR1576850     2   0.876      0.763 0.296 0.704
#&gt; SRR1576851     2   0.876      0.763 0.296 0.704
#&gt; SRR1576852     2   0.876      0.763 0.296 0.704
#&gt; SRR1576853     1   0.876      0.848 0.704 0.296
#&gt; SRR1576854     1   0.876      0.848 0.704 0.296
#&gt; SRR1576855     1   0.876      0.848 0.704 0.296
#&gt; SRR1576856     1   0.876      0.848 0.704 0.296
#&gt; SRR1576857     1   0.000      0.701 1.000 0.000
#&gt; SRR1576858     1   0.000      0.701 1.000 0.000
#&gt; SRR1576859     1   0.000      0.701 1.000 0.000
#&gt; SRR1576860     1   0.000      0.701 1.000 0.000
#&gt; SRR1576861     1   0.000      0.701 1.000 0.000
#&gt; SRR1576862     1   0.000      0.701 1.000 0.000
#&gt; SRR1576863     2   0.000      0.811 0.000 1.000
#&gt; SRR1576864     2   0.000      0.811 0.000 1.000
#&gt; SRR1576865     2   0.000      0.811 0.000 1.000
#&gt; SRR1576866     2   0.000      0.811 0.000 1.000
#&gt; SRR1576867     1   0.876      0.848 0.704 0.296
#&gt; SRR1576868     1   0.876      0.848 0.704 0.296
#&gt; SRR1576869     1   0.876      0.848 0.704 0.296
#&gt; SRR1576870     1   0.876      0.848 0.704 0.296
#&gt; SRR1576871     1   0.876      0.848 0.704 0.296
#&gt; SRR1576872     1   0.876      0.848 0.704 0.296
#&gt; SRR1576873     2   0.000      0.811 0.000 1.000
#&gt; SRR1576874     2   0.000      0.811 0.000 1.000
#&gt; SRR1576875     2   0.000      0.811 0.000 1.000
#&gt; SRR1576876     2   0.000      0.811 0.000 1.000
#&gt; SRR1576877     1   0.876      0.848 0.704 0.296
#&gt; SRR1576878     1   0.876      0.848 0.704 0.296
#&gt; SRR1576879     1   0.876      0.848 0.704 0.296
#&gt; SRR1576880     2   0.876      0.763 0.296 0.704
#&gt; SRR1576881     2   0.876      0.763 0.296 0.704
#&gt; SRR1576882     2   0.876      0.763 0.296 0.704
#&gt; SRR1576883     2   0.000      0.811 0.000 1.000
#&gt; SRR1576884     2   0.000      0.811 0.000 1.000
#&gt; SRR1576885     2   0.000      0.811 0.000 1.000
#&gt; SRR1576886     2   0.000      0.811 0.000 1.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576837     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576838     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576839     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576840     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576841     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576842     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576843     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576844     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576845     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576846     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576847     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576848     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576849     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576850     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576851     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576852     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576853     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576854     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576855     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576856     1  0.0892      0.990 0.980 0.000 0.020
#&gt; SRR1576857     3  0.3752      0.851 0.144 0.000 0.856
#&gt; SRR1576858     3  0.3752      0.851 0.144 0.000 0.856
#&gt; SRR1576859     3  0.3752      0.851 0.144 0.000 0.856
#&gt; SRR1576860     3  0.3752      0.851 0.144 0.000 0.856
#&gt; SRR1576861     3  0.3752      0.851 0.144 0.000 0.856
#&gt; SRR1576862     3  0.3752      0.851 0.144 0.000 0.856
#&gt; SRR1576863     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1576864     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1576865     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1576866     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1576867     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576877     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.991 1.000 0.000 0.000
#&gt; SRR1576880     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576881     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576882     3  0.1643      0.937 0.000 0.044 0.956
#&gt; SRR1576883     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576884     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576885     2  0.0000      0.994 0.000 1.000 0.000
#&gt; SRR1576886     2  0.0000      0.994 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576834     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576835     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576836     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576837     1  0.5217      0.753 0.608 0.012 0.380 0.000
#&gt; SRR1576838     1  0.5217      0.753 0.608 0.012 0.380 0.000
#&gt; SRR1576839     1  0.5231      0.752 0.604 0.012 0.384 0.000
#&gt; SRR1576840     1  0.5231      0.752 0.604 0.012 0.384 0.000
#&gt; SRR1576841     3  0.4843      0.837 0.000 0.000 0.604 0.396
#&gt; SRR1576842     3  0.4843      0.837 0.000 0.000 0.604 0.396
#&gt; SRR1576843     3  0.4843      0.837 0.000 0.000 0.604 0.396
#&gt; SRR1576844     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576845     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576846     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576847     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576848     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576849     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576850     4  0.1389      0.937 0.000 0.000 0.048 0.952
#&gt; SRR1576851     4  0.1389      0.937 0.000 0.000 0.048 0.952
#&gt; SRR1576852     4  0.1389      0.937 0.000 0.000 0.048 0.952
#&gt; SRR1576853     1  0.5310      0.740 0.576 0.012 0.412 0.000
#&gt; SRR1576854     1  0.5310      0.740 0.576 0.012 0.412 0.000
#&gt; SRR1576855     1  0.5310      0.740 0.576 0.012 0.412 0.000
#&gt; SRR1576856     1  0.5310      0.740 0.576 0.012 0.412 0.000
#&gt; SRR1576857     3  0.5716      0.929 0.060 0.000 0.668 0.272
#&gt; SRR1576858     3  0.5716      0.929 0.060 0.000 0.668 0.272
#&gt; SRR1576859     3  0.5716      0.929 0.060 0.000 0.668 0.272
#&gt; SRR1576860     3  0.5716      0.929 0.060 0.000 0.668 0.272
#&gt; SRR1576861     3  0.5716      0.929 0.060 0.000 0.668 0.272
#&gt; SRR1576862     3  0.5716      0.929 0.060 0.000 0.668 0.272
#&gt; SRR1576863     2  0.6465      0.617 0.004 0.588 0.332 0.076
#&gt; SRR1576864     2  0.6465      0.617 0.004 0.588 0.332 0.076
#&gt; SRR1576865     2  0.6465      0.617 0.004 0.588 0.332 0.076
#&gt; SRR1576866     2  0.6465      0.617 0.004 0.588 0.332 0.076
#&gt; SRR1576867     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576874     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576875     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576876     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576877     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.796 1.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576881     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576882     4  0.0000      0.979 0.000 0.000 0.000 1.000
#&gt; SRR1576883     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576884     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576885     2  0.0469      0.894 0.000 0.988 0.000 0.012
#&gt; SRR1576886     2  0.0469      0.894 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.2612      0.887 0.124 0.000 0.008 0.000 0.868
#&gt; SRR1576838     5  0.2612      0.887 0.124 0.000 0.008 0.000 0.868
#&gt; SRR1576839     5  0.2462      0.895 0.112 0.000 0.008 0.000 0.880
#&gt; SRR1576840     5  0.2462      0.895 0.112 0.000 0.008 0.000 0.880
#&gt; SRR1576841     3  0.3003      0.829 0.000 0.000 0.812 0.188 0.000
#&gt; SRR1576842     3  0.3003      0.829 0.000 0.000 0.812 0.188 0.000
#&gt; SRR1576843     3  0.3003      0.829 0.000 0.000 0.812 0.188 0.000
#&gt; SRR1576844     4  0.0771      0.992 0.000 0.000 0.020 0.976 0.004
#&gt; SRR1576845     4  0.0771      0.992 0.000 0.000 0.020 0.976 0.004
#&gt; SRR1576846     4  0.0771      0.992 0.000 0.000 0.020 0.976 0.004
#&gt; SRR1576847     4  0.0609      0.991 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1576848     4  0.0609      0.991 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1576849     4  0.0609      0.991 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1576850     4  0.0000      0.979 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576851     4  0.0000      0.979 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576852     4  0.0000      0.979 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576853     5  0.0955      0.922 0.028 0.000 0.004 0.000 0.968
#&gt; SRR1576854     5  0.0955      0.922 0.028 0.000 0.004 0.000 0.968
#&gt; SRR1576855     5  0.0955      0.922 0.028 0.000 0.004 0.000 0.968
#&gt; SRR1576856     5  0.0955      0.922 0.028 0.000 0.004 0.000 0.968
#&gt; SRR1576857     3  0.0290      0.925 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1576858     3  0.0290      0.925 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1576859     3  0.0290      0.925 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1576860     3  0.0290      0.925 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1576861     3  0.0290      0.925 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1576862     3  0.0290      0.925 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1576863     5  0.2424      0.894 0.000 0.052 0.008 0.032 0.908
#&gt; SRR1576864     5  0.2424      0.894 0.000 0.052 0.008 0.032 0.908
#&gt; SRR1576865     5  0.2424      0.894 0.000 0.052 0.008 0.032 0.908
#&gt; SRR1576866     5  0.2424      0.894 0.000 0.052 0.008 0.032 0.908
#&gt; SRR1576867     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0771      0.992 0.000 0.000 0.020 0.976 0.004
#&gt; SRR1576881     4  0.0771      0.992 0.000 0.000 0.020 0.976 0.004
#&gt; SRR1576882     4  0.0771      0.992 0.000 0.000 0.020 0.976 0.004
#&gt; SRR1576883     2  0.0992      0.979 0.000 0.968 0.008 0.000 0.024
#&gt; SRR1576884     2  0.0992      0.979 0.000 0.968 0.008 0.000 0.024
#&gt; SRR1576885     2  0.0992      0.979 0.000 0.968 0.008 0.000 0.024
#&gt; SRR1576886     2  0.0992      0.979 0.000 0.968 0.008 0.000 0.024
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576834     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576835     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576836     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576837     5  0.0363      0.828 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1576838     5  0.0363      0.828 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1576839     5  0.0363      0.828 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1576840     5  0.0363      0.828 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1576841     3  0.4431      0.753 0.000 0.000 0.732 0.184 0.020 0.064
#&gt; SRR1576842     3  0.4431      0.753 0.000 0.000 0.732 0.184 0.020 0.064
#&gt; SRR1576843     3  0.4431      0.753 0.000 0.000 0.732 0.184 0.020 0.064
#&gt; SRR1576844     4  0.2070      0.945 0.000 0.000 0.000 0.896 0.012 0.092
#&gt; SRR1576845     4  0.2070      0.945 0.000 0.000 0.000 0.896 0.012 0.092
#&gt; SRR1576846     4  0.2070      0.945 0.000 0.000 0.000 0.896 0.012 0.092
#&gt; SRR1576847     4  0.0000      0.944 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576848     4  0.0000      0.944 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576849     4  0.0000      0.944 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576850     4  0.0363      0.940 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; SRR1576851     4  0.0363      0.940 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; SRR1576852     4  0.0363      0.940 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; SRR1576853     5  0.3050      0.800 0.000 0.000 0.000 0.000 0.764 0.236
#&gt; SRR1576854     5  0.3050      0.800 0.000 0.000 0.000 0.000 0.764 0.236
#&gt; SRR1576855     5  0.3050      0.800 0.000 0.000 0.000 0.000 0.764 0.236
#&gt; SRR1576856     5  0.3050      0.800 0.000 0.000 0.000 0.000 0.764 0.236
#&gt; SRR1576857     3  0.0000      0.881 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      0.881 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.881 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      0.881 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      0.881 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      0.881 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     6  0.1863      1.000 0.000 0.000 0.000 0.000 0.104 0.896
#&gt; SRR1576864     6  0.1863      1.000 0.000 0.000 0.000 0.000 0.104 0.896
#&gt; SRR1576865     6  0.1863      1.000 0.000 0.000 0.000 0.000 0.104 0.896
#&gt; SRR1576866     6  0.1863      1.000 0.000 0.000 0.000 0.000 0.104 0.896
#&gt; SRR1576867     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; SRR1576874     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; SRR1576875     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; SRR1576876     2  0.0146      0.901 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; SRR1576877     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.2070      0.945 0.000 0.000 0.000 0.896 0.012 0.092
#&gt; SRR1576881     4  0.2070      0.945 0.000 0.000 0.000 0.896 0.012 0.092
#&gt; SRR1576882     4  0.2070      0.945 0.000 0.000 0.000 0.896 0.012 0.092
#&gt; SRR1576883     2  0.3101      0.771 0.000 0.756 0.000 0.000 0.000 0.244
#&gt; SRR1576884     2  0.3101      0.771 0.000 0.756 0.000 0.000 0.000 0.244
#&gt; SRR1576885     2  0.3101      0.771 0.000 0.756 0.000 0.000 0.000 0.244
#&gt; SRR1576886     2  0.3101      0.771 0.000 0.756 0.000 0.000 0.000 0.244
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.497           0.878       0.890         0.3361 0.717   0.717
#> 3 3 0.444           0.757       0.803         0.8247 0.646   0.507
#> 4 4 0.786           0.893       0.894         0.1628 0.877   0.688
#> 5 5 0.937           0.970       0.984         0.1198 0.899   0.667
#> 6 6 1.000           0.945       0.978         0.0326 0.978   0.889
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.714      0.866 0.196 0.804
#&gt; SRR1576834     2   0.714      0.866 0.196 0.804
#&gt; SRR1576835     2   0.714      0.866 0.196 0.804
#&gt; SRR1576836     2   0.714      0.866 0.196 0.804
#&gt; SRR1576837     2   0.827      0.845 0.260 0.740
#&gt; SRR1576838     2   0.827      0.845 0.260 0.740
#&gt; SRR1576839     2   0.827      0.845 0.260 0.740
#&gt; SRR1576840     2   0.827      0.845 0.260 0.740
#&gt; SRR1576841     2   0.416      0.851 0.084 0.916
#&gt; SRR1576842     2   0.416      0.851 0.084 0.916
#&gt; SRR1576843     2   0.416      0.851 0.084 0.916
#&gt; SRR1576844     2   0.000      0.849 0.000 1.000
#&gt; SRR1576845     2   0.000      0.849 0.000 1.000
#&gt; SRR1576846     2   0.000      0.849 0.000 1.000
#&gt; SRR1576847     2   0.000      0.849 0.000 1.000
#&gt; SRR1576848     2   0.000      0.849 0.000 1.000
#&gt; SRR1576849     2   0.000      0.849 0.000 1.000
#&gt; SRR1576850     2   0.000      0.849 0.000 1.000
#&gt; SRR1576851     2   0.000      0.849 0.000 1.000
#&gt; SRR1576852     2   0.000      0.849 0.000 1.000
#&gt; SRR1576853     2   0.827      0.845 0.260 0.740
#&gt; SRR1576854     2   0.827      0.845 0.260 0.740
#&gt; SRR1576855     2   0.827      0.845 0.260 0.740
#&gt; SRR1576856     2   0.827      0.845 0.260 0.740
#&gt; SRR1576857     2   0.416      0.851 0.084 0.916
#&gt; SRR1576858     2   0.416      0.851 0.084 0.916
#&gt; SRR1576859     2   0.416      0.851 0.084 0.916
#&gt; SRR1576860     2   0.416      0.851 0.084 0.916
#&gt; SRR1576861     2   0.416      0.851 0.084 0.916
#&gt; SRR1576862     2   0.416      0.851 0.084 0.916
#&gt; SRR1576863     2   0.788      0.854 0.236 0.764
#&gt; SRR1576864     2   0.781      0.856 0.232 0.768
#&gt; SRR1576865     2   0.680      0.863 0.180 0.820
#&gt; SRR1576866     2   0.680      0.863 0.180 0.820
#&gt; SRR1576867     1   0.000      1.000 1.000 0.000
#&gt; SRR1576868     1   0.000      1.000 1.000 0.000
#&gt; SRR1576869     1   0.000      1.000 1.000 0.000
#&gt; SRR1576870     1   0.000      1.000 1.000 0.000
#&gt; SRR1576871     1   0.000      1.000 1.000 0.000
#&gt; SRR1576872     1   0.000      1.000 1.000 0.000
#&gt; SRR1576873     2   0.714      0.866 0.196 0.804
#&gt; SRR1576874     2   0.714      0.866 0.196 0.804
#&gt; SRR1576875     2   0.714      0.866 0.196 0.804
#&gt; SRR1576876     2   0.714      0.866 0.196 0.804
#&gt; SRR1576877     1   0.000      1.000 1.000 0.000
#&gt; SRR1576878     1   0.000      1.000 1.000 0.000
#&gt; SRR1576879     1   0.000      1.000 1.000 0.000
#&gt; SRR1576880     2   0.000      0.849 0.000 1.000
#&gt; SRR1576881     2   0.000      0.849 0.000 1.000
#&gt; SRR1576882     2   0.000      0.849 0.000 1.000
#&gt; SRR1576883     2   0.714      0.866 0.196 0.804
#&gt; SRR1576884     2   0.714      0.866 0.196 0.804
#&gt; SRR1576885     2   0.714      0.866 0.196 0.804
#&gt; SRR1576886     2   0.714      0.866 0.196 0.804
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576837     2  0.2796      0.854 0.092 0.908 0.000
#&gt; SRR1576838     2  0.2796      0.854 0.092 0.908 0.000
#&gt; SRR1576839     2  0.2796      0.854 0.092 0.908 0.000
#&gt; SRR1576840     2  0.2796      0.854 0.092 0.908 0.000
#&gt; SRR1576841     3  0.5948      0.510 0.000 0.360 0.640
#&gt; SRR1576842     3  0.5948      0.510 0.000 0.360 0.640
#&gt; SRR1576843     3  0.5948      0.510 0.000 0.360 0.640
#&gt; SRR1576844     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576845     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576846     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576847     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576848     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576849     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576850     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576851     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576852     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576853     2  0.3412      0.836 0.124 0.876 0.000
#&gt; SRR1576854     2  0.3551      0.831 0.132 0.868 0.000
#&gt; SRR1576855     2  0.4399      0.779 0.188 0.812 0.000
#&gt; SRR1576856     2  0.4702      0.753 0.212 0.788 0.000
#&gt; SRR1576857     3  0.6717      0.519 0.020 0.352 0.628
#&gt; SRR1576858     3  0.6717      0.519 0.020 0.352 0.628
#&gt; SRR1576859     3  0.6717      0.519 0.020 0.352 0.628
#&gt; SRR1576860     3  0.6717      0.519 0.020 0.352 0.628
#&gt; SRR1576861     3  0.6717      0.519 0.020 0.352 0.628
#&gt; SRR1576862     3  0.6717      0.519 0.020 0.352 0.628
#&gt; SRR1576863     3  0.8040      0.401 0.092 0.300 0.608
#&gt; SRR1576864     3  0.8089      0.384 0.092 0.308 0.600
#&gt; SRR1576865     2  0.8346      0.251 0.092 0.548 0.360
#&gt; SRR1576866     2  0.8346      0.251 0.092 0.548 0.360
#&gt; SRR1576867     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576868     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576869     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576870     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576871     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576872     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576873     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.878 0.000 1.000 0.000
#&gt; SRR1576877     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576878     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576879     1  0.0892      1.000 0.980 0.020 0.000
#&gt; SRR1576880     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576881     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576882     3  0.3482      0.732 0.000 0.128 0.872
#&gt; SRR1576883     2  0.0592      0.873 0.000 0.988 0.012
#&gt; SRR1576884     2  0.0592      0.873 0.000 0.988 0.012
#&gt; SRR1576885     2  0.0592      0.873 0.000 0.988 0.012
#&gt; SRR1576886     2  0.0592      0.873 0.000 0.988 0.012
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576834     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576835     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576836     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576837     2  0.0000      0.844 0.000 1.000 0.000 0.000
#&gt; SRR1576838     2  0.0000      0.844 0.000 1.000 0.000 0.000
#&gt; SRR1576839     2  0.0000      0.844 0.000 1.000 0.000 0.000
#&gt; SRR1576840     2  0.0000      0.844 0.000 1.000 0.000 0.000
#&gt; SRR1576841     4  0.3311      0.755 0.000 0.000 0.172 0.828
#&gt; SRR1576842     4  0.3311      0.755 0.000 0.000 0.172 0.828
#&gt; SRR1576843     4  0.3311      0.755 0.000 0.000 0.172 0.828
#&gt; SRR1576844     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576845     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576846     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576847     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576848     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576849     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576850     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576851     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576852     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576853     2  0.1716      0.818 0.000 0.936 0.000 0.064
#&gt; SRR1576854     2  0.1716      0.818 0.000 0.936 0.000 0.064
#&gt; SRR1576855     2  0.0469      0.841 0.012 0.988 0.000 0.000
#&gt; SRR1576856     2  0.1211      0.829 0.040 0.960 0.000 0.000
#&gt; SRR1576857     3  0.3356      1.000 0.000 0.000 0.824 0.176
#&gt; SRR1576858     3  0.3356      1.000 0.000 0.000 0.824 0.176
#&gt; SRR1576859     3  0.3356      1.000 0.000 0.000 0.824 0.176
#&gt; SRR1576860     3  0.3356      1.000 0.000 0.000 0.824 0.176
#&gt; SRR1576861     3  0.3356      1.000 0.000 0.000 0.824 0.176
#&gt; SRR1576862     3  0.3356      1.000 0.000 0.000 0.824 0.176
#&gt; SRR1576863     2  0.4605      0.492 0.000 0.664 0.000 0.336
#&gt; SRR1576864     2  0.4585      0.500 0.000 0.668 0.000 0.332
#&gt; SRR1576865     2  0.3172      0.743 0.000 0.840 0.000 0.160
#&gt; SRR1576866     2  0.3172      0.743 0.000 0.840 0.000 0.160
#&gt; SRR1576867     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576874     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576875     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576876     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576877     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576881     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576882     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; SRR1576883     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576884     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576885     2  0.3808      0.865 0.000 0.812 0.176 0.012
#&gt; SRR1576886     2  0.3808      0.865 0.000 0.812 0.176 0.012
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5
#&gt; SRR1576833     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576834     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576835     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576836     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576837     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576838     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576839     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576840     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576841     4   0.233      0.872  0 0.000  0 0.876 0.124
#&gt; SRR1576842     4   0.233      0.872  0 0.000  0 0.876 0.124
#&gt; SRR1576843     4   0.233      0.872  0 0.000  0 0.876 0.124
#&gt; SRR1576844     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576845     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576846     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576847     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576848     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576849     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576850     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576851     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576852     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576853     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576854     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576855     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576856     5   0.000      0.945  0 0.000  0 0.000 1.000
#&gt; SRR1576857     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1576858     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1576859     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1576860     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1576861     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1576862     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1576863     5   0.251      0.887  0 0.008  0 0.116 0.876
#&gt; SRR1576864     5   0.251      0.887  0 0.008  0 0.116 0.876
#&gt; SRR1576865     5   0.251      0.887  0 0.008  0 0.116 0.876
#&gt; SRR1576866     5   0.251      0.887  0 0.008  0 0.116 0.876
#&gt; SRR1576867     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576868     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576869     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576870     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576871     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576872     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576873     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576874     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576875     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576876     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576877     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576878     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576879     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1576880     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576881     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576882     4   0.000      0.970  0 0.000  0 1.000 0.000
#&gt; SRR1576883     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576884     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576885     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1576886     2   0.000      1.000  0 1.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3    p4    p5 p6
#&gt; SRR1576833     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576834     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576835     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576836     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576837     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576838     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576839     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576840     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576841     4   0.377      0.419  0  0  0 0.596 0.404  0
#&gt; SRR1576842     4   0.377      0.419  0  0  0 0.596 0.404  0
#&gt; SRR1576843     4   0.377      0.419  0  0  0 0.596 0.404  0
#&gt; SRR1576844     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576845     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576846     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576847     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576848     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576849     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576850     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576851     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576852     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576853     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576854     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576855     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576856     5   0.000      1.000  0  0  0 0.000 1.000  0
#&gt; SRR1576857     3   0.000      1.000  0  0  1 0.000 0.000  0
#&gt; SRR1576858     3   0.000      1.000  0  0  1 0.000 0.000  0
#&gt; SRR1576859     3   0.000      1.000  0  0  1 0.000 0.000  0
#&gt; SRR1576860     3   0.000      1.000  0  0  1 0.000 0.000  0
#&gt; SRR1576861     3   0.000      1.000  0  0  1 0.000 0.000  0
#&gt; SRR1576862     3   0.000      1.000  0  0  1 0.000 0.000  0
#&gt; SRR1576863     6   0.000      1.000  0  0  0 0.000 0.000  1
#&gt; SRR1576864     6   0.000      1.000  0  0  0 0.000 0.000  1
#&gt; SRR1576865     6   0.000      1.000  0  0  0 0.000 0.000  1
#&gt; SRR1576866     6   0.000      1.000  0  0  0 0.000 0.000  1
#&gt; SRR1576867     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576868     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576869     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576870     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576871     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576872     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576873     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576874     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576875     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576876     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576877     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576878     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576879     1   0.000      1.000  1  0  0 0.000 0.000  0
#&gt; SRR1576880     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576881     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576882     4   0.000      0.899  0  0  0 1.000 0.000  0
#&gt; SRR1576883     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576884     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576885     2   0.000      1.000  0  1  0 0.000 0.000  0
#&gt; SRR1576886     2   0.000      1.000  0  1  0 0.000 0.000  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.302           0.672       0.749         0.4242 0.560   0.560
#> 3 3 0.415           0.593       0.741         0.4141 0.790   0.626
#> 4 4 0.627           0.711       0.807         0.1221 0.672   0.330
#> 5 5 0.868           0.912       0.934         0.1792 0.899   0.667
#> 6 6 0.942           0.888       0.941         0.0473 0.969   0.847
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576834     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576835     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576836     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576837     2  0.5842      0.486 0.140 0.860
#&gt; SRR1576838     2  0.5842      0.486 0.140 0.860
#&gt; SRR1576839     2  0.5629      0.504 0.132 0.868
#&gt; SRR1576840     2  0.5629      0.504 0.132 0.868
#&gt; SRR1576841     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576842     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576843     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576844     2  0.0000      0.648 0.000 1.000
#&gt; SRR1576845     2  0.0000      0.648 0.000 1.000
#&gt; SRR1576846     2  0.0000      0.648 0.000 1.000
#&gt; SRR1576847     2  0.0938      0.648 0.012 0.988
#&gt; SRR1576848     2  0.1184      0.648 0.016 0.984
#&gt; SRR1576849     2  0.0938      0.648 0.012 0.988
#&gt; SRR1576850     2  0.7602      0.462 0.220 0.780
#&gt; SRR1576851     2  0.7602      0.462 0.220 0.780
#&gt; SRR1576852     2  0.7602      0.462 0.220 0.780
#&gt; SRR1576853     1  0.9427      0.866 0.640 0.360
#&gt; SRR1576854     1  0.9427      0.866 0.640 0.360
#&gt; SRR1576855     1  0.9286      0.868 0.656 0.344
#&gt; SRR1576856     1  0.9286      0.868 0.656 0.344
#&gt; SRR1576857     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576858     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576859     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576860     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576861     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576862     2  0.4161      0.625 0.084 0.916
#&gt; SRR1576863     1  0.9580      0.855 0.620 0.380
#&gt; SRR1576864     1  0.9580      0.855 0.620 0.380
#&gt; SRR1576865     1  0.9580      0.855 0.620 0.380
#&gt; SRR1576866     1  0.9580      0.855 0.620 0.380
#&gt; SRR1576867     1  0.9954      0.880 0.540 0.460
#&gt; SRR1576868     1  0.9954      0.880 0.540 0.460
#&gt; SRR1576869     1  0.9954      0.880 0.540 0.460
#&gt; SRR1576870     1  0.9954      0.880 0.540 0.460
#&gt; SRR1576871     1  0.9954      0.880 0.540 0.460
#&gt; SRR1576872     1  0.9954      0.880 0.540 0.460
#&gt; SRR1576873     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576874     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576875     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576876     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576877     1  0.9996      0.855 0.512 0.488
#&gt; SRR1576878     1  0.9996      0.855 0.512 0.488
#&gt; SRR1576879     1  0.9996      0.855 0.512 0.488
#&gt; SRR1576880     2  0.0000      0.648 0.000 1.000
#&gt; SRR1576881     2  0.0000      0.648 0.000 1.000
#&gt; SRR1576882     2  0.0000      0.648 0.000 1.000
#&gt; SRR1576883     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576884     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576885     2  0.9795      0.562 0.416 0.584
#&gt; SRR1576886     2  0.9795      0.562 0.416 0.584
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576834     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576835     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576836     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576837     3  0.6825      0.165 0.488 0.012 0.500
#&gt; SRR1576838     3  0.6825      0.165 0.488 0.012 0.500
#&gt; SRR1576839     3  0.6825      0.165 0.488 0.012 0.500
#&gt; SRR1576840     3  0.6825      0.165 0.488 0.012 0.500
#&gt; SRR1576841     3  0.0000      0.622 0.000 0.000 1.000
#&gt; SRR1576842     3  0.0000      0.622 0.000 0.000 1.000
#&gt; SRR1576843     3  0.0000      0.622 0.000 0.000 1.000
#&gt; SRR1576844     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576845     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576846     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576847     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576848     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576849     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576850     3  0.3482      0.608 0.128 0.000 0.872
#&gt; SRR1576851     3  0.3482      0.608 0.128 0.000 0.872
#&gt; SRR1576852     3  0.3482      0.608 0.128 0.000 0.872
#&gt; SRR1576853     1  0.6381      0.284 0.648 0.012 0.340
#&gt; SRR1576854     1  0.6381      0.284 0.648 0.012 0.340
#&gt; SRR1576855     1  0.5835      0.293 0.660 0.000 0.340
#&gt; SRR1576856     1  0.5835      0.293 0.660 0.000 0.340
#&gt; SRR1576857     3  0.9258      0.400 0.216 0.256 0.528
#&gt; SRR1576858     3  0.9258      0.400 0.216 0.256 0.528
#&gt; SRR1576859     3  0.9258      0.400 0.216 0.256 0.528
#&gt; SRR1576860     3  0.9258      0.400 0.216 0.256 0.528
#&gt; SRR1576861     3  0.9258      0.400 0.216 0.256 0.528
#&gt; SRR1576862     3  0.9258      0.400 0.216 0.256 0.528
#&gt; SRR1576863     1  0.9281      0.151 0.488 0.172 0.340
#&gt; SRR1576864     1  0.9281      0.151 0.488 0.172 0.340
#&gt; SRR1576865     1  0.9281      0.151 0.488 0.172 0.340
#&gt; SRR1576866     1  0.9281      0.151 0.488 0.172 0.340
#&gt; SRR1576867     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576868     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576869     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576870     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576871     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576872     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576873     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576874     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576875     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576876     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576877     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576878     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576879     1  0.4504      0.654 0.804 0.196 0.000
#&gt; SRR1576880     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576881     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576882     3  0.0237      0.624 0.004 0.000 0.996
#&gt; SRR1576883     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576884     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576885     2  0.6267      1.000 0.000 0.548 0.452
#&gt; SRR1576886     2  0.6267      1.000 0.000 0.548 0.452
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4
#&gt; SRR1576833     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576834     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576835     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576836     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576837     4  0.3402      0.744 0.004 0.164  0 0.832
#&gt; SRR1576838     4  0.3402      0.744 0.004 0.164  0 0.832
#&gt; SRR1576839     4  0.3402      0.744 0.004 0.164  0 0.832
#&gt; SRR1576840     4  0.3402      0.744 0.004 0.164  0 0.832
#&gt; SRR1576841     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576842     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576843     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576844     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576845     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576846     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576847     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576848     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576849     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576850     4  0.4817      0.303 0.000 0.388  0 0.612
#&gt; SRR1576851     4  0.4817      0.303 0.000 0.388  0 0.612
#&gt; SRR1576852     4  0.4817      0.303 0.000 0.388  0 0.612
#&gt; SRR1576853     4  0.0188      0.787 0.004 0.000  0 0.996
#&gt; SRR1576854     4  0.0188      0.787 0.004 0.000  0 0.996
#&gt; SRR1576855     4  0.0188      0.787 0.004 0.000  0 0.996
#&gt; SRR1576856     4  0.0188      0.787 0.004 0.000  0 0.996
#&gt; SRR1576857     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; SRR1576858     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; SRR1576859     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; SRR1576860     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; SRR1576861     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; SRR1576862     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; SRR1576863     4  0.0000      0.789 0.000 0.000  0 1.000
#&gt; SRR1576864     4  0.0000      0.789 0.000 0.000  0 1.000
#&gt; SRR1576865     4  0.0000      0.789 0.000 0.000  0 1.000
#&gt; SRR1576866     4  0.0000      0.789 0.000 0.000  0 1.000
#&gt; SRR1576867     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576868     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576869     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576870     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576871     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576872     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576873     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576874     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576875     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576876     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576877     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576878     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576879     1  0.3688      1.000 0.792 0.000  0 0.208
#&gt; SRR1576880     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576881     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576882     2  0.7591      0.461 0.208 0.452  0 0.340
#&gt; SRR1576883     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576884     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576885     2  0.0000      0.639 0.000 1.000  0 0.000
#&gt; SRR1576886     2  0.0000      0.639 0.000 1.000  0 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5
#&gt; SRR1576833     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576834     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576835     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576836     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576837     5   0.578      0.712 0.212 0.000  0 0.172 0.616
#&gt; SRR1576838     5   0.578      0.712 0.212 0.000  0 0.172 0.616
#&gt; SRR1576839     5   0.578      0.712 0.212 0.000  0 0.172 0.616
#&gt; SRR1576840     5   0.578      0.712 0.212 0.000  0 0.172 0.616
#&gt; SRR1576841     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576842     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576843     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576844     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576845     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576846     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576847     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576848     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576849     4   0.000      0.994 0.000 0.000  0 1.000 0.000
#&gt; SRR1576850     5   0.572      0.539 0.000 0.128  0 0.268 0.604
#&gt; SRR1576851     5   0.572      0.539 0.000 0.128  0 0.268 0.604
#&gt; SRR1576852     5   0.572      0.539 0.000 0.128  0 0.268 0.604
#&gt; SRR1576853     5   0.304      0.730 0.192 0.000  0 0.000 0.808
#&gt; SRR1576854     5   0.304      0.730 0.192 0.000  0 0.000 0.808
#&gt; SRR1576855     5   0.304      0.730 0.192 0.000  0 0.000 0.808
#&gt; SRR1576856     5   0.304      0.730 0.192 0.000  0 0.000 0.808
#&gt; SRR1576857     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1576858     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1576859     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1576860     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1576861     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1576862     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1576863     5   0.000      0.738 0.000 0.000  0 0.000 1.000
#&gt; SRR1576864     5   0.000      0.738 0.000 0.000  0 0.000 1.000
#&gt; SRR1576865     5   0.000      0.738 0.000 0.000  0 0.000 1.000
#&gt; SRR1576866     5   0.000      0.738 0.000 0.000  0 0.000 1.000
#&gt; SRR1576867     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576868     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576869     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576870     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576871     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576872     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576873     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576874     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576875     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576876     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576877     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576878     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576879     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1576880     4   0.051      0.983 0.000 0.016  0 0.984 0.000
#&gt; SRR1576881     4   0.051      0.983 0.000 0.016  0 0.984 0.000
#&gt; SRR1576882     4   0.051      0.983 0.000 0.016  0 0.984 0.000
#&gt; SRR1576883     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576884     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576885     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1576886     2   0.000      1.000 0.000 1.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576837     5  0.0000      0.721 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1576838     5  0.0000      0.721 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1576839     5  0.0000      0.721 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1576840     5  0.0000      0.721 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1576841     4  0.1141      0.951 0.000 0.000  0 0.948 0.052 0.000
#&gt; SRR1576842     4  0.1141      0.951 0.000 0.000  0 0.948 0.052 0.000
#&gt; SRR1576843     4  0.1141      0.951 0.000 0.000  0 0.948 0.052 0.000
#&gt; SRR1576844     4  0.0000      0.979 0.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1576845     4  0.0000      0.979 0.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1576846     4  0.0000      0.979 0.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1576847     4  0.0000      0.979 0.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1576848     4  0.0000      0.979 0.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1576849     4  0.0000      0.979 0.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1576850     5  0.6689      0.298 0.000 0.120  0 0.104 0.488 0.288
#&gt; SRR1576851     5  0.6689      0.298 0.000 0.120  0 0.104 0.488 0.288
#&gt; SRR1576852     5  0.6689      0.298 0.000 0.120  0 0.104 0.488 0.288
#&gt; SRR1576853     5  0.2823      0.672 0.000 0.000  0 0.000 0.796 0.204
#&gt; SRR1576854     5  0.2823      0.672 0.000 0.000  0 0.000 0.796 0.204
#&gt; SRR1576855     5  0.2823      0.672 0.000 0.000  0 0.000 0.796 0.204
#&gt; SRR1576856     5  0.2823      0.672 0.000 0.000  0 0.000 0.796 0.204
#&gt; SRR1576857     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1576863     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1576864     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1576865     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1576866     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1576867     1  0.0000      0.908 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.908 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.908 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.908 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.908 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.908 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576877     1  0.2883      0.801 0.788 0.000  0 0.000 0.212 0.000
#&gt; SRR1576878     1  0.2883      0.801 0.788 0.000  0 0.000 0.212 0.000
#&gt; SRR1576879     1  0.2883      0.801 0.788 0.000  0 0.000 0.212 0.000
#&gt; SRR1576880     4  0.0458      0.971 0.000 0.016  0 0.984 0.000 0.000
#&gt; SRR1576881     4  0.0458      0.971 0.000 0.016  0 0.984 0.000 0.000
#&gt; SRR1576882     4  0.0458      0.971 0.000 0.016  0 0.984 0.000 0.000
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.287           0.679       0.812         0.4499 0.516   0.516
#> 3 3 0.732           0.873       0.941         0.3518 0.761   0.580
#> 4 4 0.885           0.883       0.939         0.2396 0.831   0.577
#> 5 5 0.838           0.853       0.904         0.0661 0.834   0.454
#> 6 6 0.929           0.880       0.929         0.0257 0.920   0.648
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.0376      0.794 0.004 0.996
#&gt; SRR1576834     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576835     2  0.6712      0.787 0.176 0.824
#&gt; SRR1576836     2  0.6623      0.787 0.172 0.828
#&gt; SRR1576837     1  0.9996      0.526 0.512 0.488
#&gt; SRR1576838     1  1.0000      0.513 0.504 0.496
#&gt; SRR1576839     2  0.9998     -0.238 0.492 0.508
#&gt; SRR1576840     1  1.0000      0.193 0.504 0.496
#&gt; SRR1576841     1  0.8909      0.471 0.692 0.308
#&gt; SRR1576842     1  0.8955      0.464 0.688 0.312
#&gt; SRR1576843     1  0.8955      0.464 0.688 0.312
#&gt; SRR1576844     2  0.7815      0.754 0.232 0.768
#&gt; SRR1576845     2  0.7815      0.754 0.232 0.768
#&gt; SRR1576846     2  0.7815      0.754 0.232 0.768
#&gt; SRR1576847     2  0.8081      0.742 0.248 0.752
#&gt; SRR1576848     2  0.8081      0.742 0.248 0.752
#&gt; SRR1576849     2  0.8081      0.742 0.248 0.752
#&gt; SRR1576850     2  0.7299      0.777 0.204 0.796
#&gt; SRR1576851     2  0.7299      0.777 0.204 0.796
#&gt; SRR1576852     2  0.7299      0.777 0.204 0.796
#&gt; SRR1576853     2  0.4298      0.706 0.088 0.912
#&gt; SRR1576854     2  0.4431      0.701 0.092 0.908
#&gt; SRR1576855     2  0.4690      0.677 0.100 0.900
#&gt; SRR1576856     2  0.4939      0.667 0.108 0.892
#&gt; SRR1576857     1  0.7745      0.587 0.772 0.228
#&gt; SRR1576858     1  0.7745      0.587 0.772 0.228
#&gt; SRR1576859     1  0.7745      0.587 0.772 0.228
#&gt; SRR1576860     1  0.5842      0.636 0.860 0.140
#&gt; SRR1576861     1  0.5842      0.636 0.860 0.140
#&gt; SRR1576862     1  0.5842      0.636 0.860 0.140
#&gt; SRR1576863     2  0.0376      0.790 0.004 0.996
#&gt; SRR1576864     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576865     2  0.1184      0.779 0.016 0.984
#&gt; SRR1576866     2  0.1184      0.779 0.016 0.984
#&gt; SRR1576867     1  0.7883      0.684 0.764 0.236
#&gt; SRR1576868     1  0.7883      0.684 0.764 0.236
#&gt; SRR1576869     1  0.7883      0.684 0.764 0.236
#&gt; SRR1576870     1  0.7883      0.684 0.764 0.236
#&gt; SRR1576871     1  0.7883      0.684 0.764 0.236
#&gt; SRR1576872     1  0.7883      0.684 0.764 0.236
#&gt; SRR1576873     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576874     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576875     2  0.5059      0.796 0.112 0.888
#&gt; SRR1576876     2  0.5178      0.796 0.116 0.884
#&gt; SRR1576877     1  0.9000      0.649 0.684 0.316
#&gt; SRR1576878     1  0.9000      0.649 0.684 0.316
#&gt; SRR1576879     1  0.9000      0.649 0.684 0.316
#&gt; SRR1576880     2  0.7299      0.777 0.204 0.796
#&gt; SRR1576881     2  0.7453      0.771 0.212 0.788
#&gt; SRR1576882     2  0.7299      0.777 0.204 0.796
#&gt; SRR1576883     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576884     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576885     2  0.0000      0.793 0.000 1.000
#&gt; SRR1576886     2  0.0000      0.793 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576837     1  0.0237      0.905 0.996 0.004 0.000
#&gt; SRR1576838     1  0.0424      0.902 0.992 0.008 0.000
#&gt; SRR1576839     2  0.1337      0.927 0.016 0.972 0.012
#&gt; SRR1576840     2  0.1751      0.921 0.012 0.960 0.028
#&gt; SRR1576841     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576842     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576843     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576844     2  0.2625      0.904 0.000 0.916 0.084
#&gt; SRR1576845     2  0.2537      0.906 0.000 0.920 0.080
#&gt; SRR1576846     2  0.2448      0.908 0.000 0.924 0.076
#&gt; SRR1576847     3  0.4654      0.755 0.000 0.208 0.792
#&gt; SRR1576848     3  0.4654      0.755 0.000 0.208 0.792
#&gt; SRR1576849     3  0.4654      0.755 0.000 0.208 0.792
#&gt; SRR1576850     2  0.3619      0.868 0.000 0.864 0.136
#&gt; SRR1576851     2  0.3879      0.854 0.000 0.848 0.152
#&gt; SRR1576852     2  0.3551      0.871 0.000 0.868 0.132
#&gt; SRR1576853     2  0.5443      0.661 0.260 0.736 0.004
#&gt; SRR1576854     2  0.5443      0.661 0.260 0.736 0.004
#&gt; SRR1576855     1  0.6451      0.183 0.560 0.436 0.004
#&gt; SRR1576856     1  0.6169      0.403 0.636 0.360 0.004
#&gt; SRR1576857     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576858     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576859     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576860     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576861     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576862     3  0.0237      0.922 0.004 0.000 0.996
#&gt; SRR1576863     2  0.0237      0.935 0.000 0.996 0.004
#&gt; SRR1576864     2  0.0237      0.935 0.000 0.996 0.004
#&gt; SRR1576865     2  0.0237      0.935 0.000 0.996 0.004
#&gt; SRR1576866     2  0.0237      0.935 0.000 0.996 0.004
#&gt; SRR1576867     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576877     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.908 1.000 0.000 0.000
#&gt; SRR1576880     2  0.3619      0.869 0.000 0.864 0.136
#&gt; SRR1576881     2  0.3816      0.858 0.000 0.852 0.148
#&gt; SRR1576882     2  0.3816      0.858 0.000 0.852 0.148
#&gt; SRR1576883     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576884     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576885     2  0.0000      0.936 0.000 1.000 0.000
#&gt; SRR1576886     2  0.0000      0.936 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576837     1  0.0992      0.978 0.976 0.012 0.004 0.008
#&gt; SRR1576838     1  0.0992      0.978 0.976 0.012 0.004 0.008
#&gt; SRR1576839     4  0.8024      0.136 0.384 0.020 0.172 0.424
#&gt; SRR1576840     4  0.8486      0.166 0.316 0.024 0.276 0.384
#&gt; SRR1576841     3  0.0469      0.979 0.000 0.000 0.988 0.012
#&gt; SRR1576842     3  0.0469      0.979 0.000 0.000 0.988 0.012
#&gt; SRR1576843     3  0.0469      0.979 0.000 0.000 0.988 0.012
#&gt; SRR1576844     2  0.4225      0.781 0.000 0.792 0.184 0.024
#&gt; SRR1576845     2  0.4225      0.781 0.000 0.792 0.184 0.024
#&gt; SRR1576846     2  0.4139      0.787 0.000 0.800 0.176 0.024
#&gt; SRR1576847     3  0.1833      0.953 0.000 0.032 0.944 0.024
#&gt; SRR1576848     3  0.1733      0.957 0.000 0.028 0.948 0.024
#&gt; SRR1576849     3  0.1733      0.957 0.000 0.028 0.948 0.024
#&gt; SRR1576850     4  0.0336      0.895 0.000 0.008 0.000 0.992
#&gt; SRR1576851     4  0.0336      0.895 0.000 0.008 0.000 0.992
#&gt; SRR1576852     4  0.0336      0.895 0.000 0.008 0.000 0.992
#&gt; SRR1576853     4  0.0927      0.897 0.008 0.016 0.000 0.976
#&gt; SRR1576854     4  0.0927      0.897 0.008 0.016 0.000 0.976
#&gt; SRR1576855     4  0.0927      0.897 0.008 0.016 0.000 0.976
#&gt; SRR1576856     4  0.0927      0.897 0.008 0.016 0.000 0.976
#&gt; SRR1576857     3  0.0188      0.980 0.000 0.000 0.996 0.004
#&gt; SRR1576858     3  0.0188      0.980 0.000 0.000 0.996 0.004
#&gt; SRR1576859     3  0.0188      0.980 0.000 0.000 0.996 0.004
#&gt; SRR1576860     3  0.0188      0.980 0.000 0.000 0.996 0.004
#&gt; SRR1576861     3  0.0188      0.980 0.000 0.000 0.996 0.004
#&gt; SRR1576862     3  0.0188      0.980 0.000 0.000 0.996 0.004
#&gt; SRR1576863     4  0.0592      0.899 0.000 0.016 0.000 0.984
#&gt; SRR1576864     4  0.0592      0.899 0.000 0.016 0.000 0.984
#&gt; SRR1576865     4  0.0592      0.899 0.000 0.016 0.000 0.984
#&gt; SRR1576866     4  0.0592      0.899 0.000 0.016 0.000 0.984
#&gt; SRR1576867     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576877     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.995 1.000 0.000 0.000 0.000
#&gt; SRR1576880     2  0.5271      0.613 0.000 0.656 0.320 0.024
#&gt; SRR1576881     2  0.5311      0.600 0.000 0.648 0.328 0.024
#&gt; SRR1576882     2  0.5311      0.600 0.000 0.648 0.328 0.024
#&gt; SRR1576883     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      0.898 0.000 1.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      0.898 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     1  0.6549      0.299 0.496 0.012 0.152 0.000 0.340
#&gt; SRR1576838     1  0.6608      0.300 0.496 0.016 0.148 0.000 0.340
#&gt; SRR1576839     3  0.5931      0.616 0.040 0.220 0.652 0.000 0.088
#&gt; SRR1576840     3  0.5435      0.609 0.020 0.244 0.668 0.000 0.068
#&gt; SRR1576841     4  0.3480      0.675 0.000 0.000 0.248 0.752 0.000
#&gt; SRR1576842     4  0.3452      0.680 0.000 0.000 0.244 0.756 0.000
#&gt; SRR1576843     4  0.3480      0.675 0.000 0.000 0.248 0.752 0.000
#&gt; SRR1576844     4  0.0000      0.826 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576845     4  0.0162      0.826 0.000 0.000 0.004 0.996 0.000
#&gt; SRR1576846     4  0.0162      0.826 0.000 0.000 0.004 0.996 0.000
#&gt; SRR1576847     4  0.2020      0.808 0.000 0.000 0.100 0.900 0.000
#&gt; SRR1576848     4  0.2020      0.808 0.000 0.000 0.100 0.900 0.000
#&gt; SRR1576849     4  0.2020      0.808 0.000 0.000 0.100 0.900 0.000
#&gt; SRR1576850     4  0.3336      0.724 0.000 0.000 0.000 0.772 0.228
#&gt; SRR1576851     4  0.3366      0.720 0.000 0.000 0.000 0.768 0.232
#&gt; SRR1576852     4  0.3366      0.720 0.000 0.000 0.000 0.768 0.232
#&gt; SRR1576853     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576854     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576855     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576856     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576857     3  0.2471      0.880 0.000 0.000 0.864 0.136 0.000
#&gt; SRR1576858     3  0.2471      0.880 0.000 0.000 0.864 0.136 0.000
#&gt; SRR1576859     3  0.2471      0.880 0.000 0.000 0.864 0.136 0.000
#&gt; SRR1576860     3  0.2471      0.880 0.000 0.000 0.864 0.136 0.000
#&gt; SRR1576861     3  0.2471      0.880 0.000 0.000 0.864 0.136 0.000
#&gt; SRR1576862     3  0.2471      0.880 0.000 0.000 0.864 0.136 0.000
#&gt; SRR1576863     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576864     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576865     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576866     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576867     1  0.0000      0.854 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.854 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.854 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.854 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.854 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.854 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.3146      0.802 0.844 0.000 0.128 0.028 0.000
#&gt; SRR1576878     1  0.3146      0.802 0.844 0.000 0.128 0.028 0.000
#&gt; SRR1576879     1  0.3146      0.802 0.844 0.000 0.128 0.028 0.000
#&gt; SRR1576880     4  0.2280      0.789 0.000 0.000 0.120 0.880 0.000
#&gt; SRR1576881     4  0.2280      0.789 0.000 0.000 0.120 0.880 0.000
#&gt; SRR1576882     4  0.2280      0.789 0.000 0.000 0.120 0.880 0.000
#&gt; SRR1576883     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576885     2  0.0162      0.996 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576886     2  0.0162      0.996 0.000 0.996 0.004 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0146      0.916 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576834     2  0.0146      0.916 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576835     2  0.0000      0.917 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.917 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     1  0.4562      0.751 0.776 0.028 0.056 0.000 0.100 0.040
#&gt; SRR1576838     1  0.4692      0.744 0.768 0.032 0.060 0.000 0.100 0.040
#&gt; SRR1576839     2  0.6910      0.425 0.060 0.544 0.208 0.000 0.148 0.040
#&gt; SRR1576840     2  0.6860      0.418 0.056 0.540 0.232 0.000 0.132 0.040
#&gt; SRR1576841     3  0.3563      0.621 0.000 0.000 0.664 0.336 0.000 0.000
#&gt; SRR1576842     3  0.3547      0.627 0.000 0.000 0.668 0.332 0.000 0.000
#&gt; SRR1576843     3  0.3592      0.608 0.000 0.000 0.656 0.344 0.000 0.000
#&gt; SRR1576844     4  0.0603      0.961 0.000 0.000 0.016 0.980 0.000 0.004
#&gt; SRR1576845     4  0.0603      0.961 0.000 0.000 0.016 0.980 0.000 0.004
#&gt; SRR1576846     4  0.0603      0.961 0.000 0.000 0.016 0.980 0.000 0.004
#&gt; SRR1576847     4  0.0858      0.957 0.000 0.000 0.028 0.968 0.000 0.004
#&gt; SRR1576848     4  0.0858      0.957 0.000 0.000 0.028 0.968 0.000 0.004
#&gt; SRR1576849     4  0.0858      0.957 0.000 0.000 0.028 0.968 0.000 0.004
#&gt; SRR1576850     4  0.1297      0.944 0.000 0.000 0.000 0.948 0.040 0.012
#&gt; SRR1576851     4  0.1297      0.944 0.000 0.000 0.000 0.948 0.040 0.012
#&gt; SRR1576852     4  0.1297      0.944 0.000 0.000 0.000 0.948 0.040 0.012
#&gt; SRR1576853     5  0.0291      0.936 0.000 0.004 0.000 0.000 0.992 0.004
#&gt; SRR1576854     5  0.0291      0.936 0.000 0.004 0.000 0.000 0.992 0.004
#&gt; SRR1576855     5  0.0146      0.937 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1576856     5  0.0146      0.937 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1576857     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     5  0.1918      0.937 0.000 0.000 0.000 0.008 0.904 0.088
#&gt; SRR1576864     5  0.1918      0.937 0.000 0.000 0.000 0.008 0.904 0.088
#&gt; SRR1576865     5  0.1918      0.937 0.000 0.000 0.000 0.008 0.904 0.088
#&gt; SRR1576866     5  0.1918      0.937 0.000 0.000 0.000 0.008 0.904 0.088
#&gt; SRR1576867     1  0.0146      0.915 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1576868     1  0.0146      0.915 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1576869     1  0.0146      0.915 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1576870     1  0.0000      0.916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.917 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.917 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.917 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.917 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576877     6  0.2402      1.000 0.140 0.000 0.000 0.004 0.000 0.856
#&gt; SRR1576878     6  0.2402      1.000 0.140 0.000 0.000 0.004 0.000 0.856
#&gt; SRR1576879     6  0.2402      1.000 0.140 0.000 0.000 0.004 0.000 0.856
#&gt; SRR1576880     4  0.1349      0.944 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576881     4  0.1285      0.947 0.000 0.000 0.004 0.944 0.000 0.052
#&gt; SRR1576882     4  0.1285      0.947 0.000 0.000 0.004 0.944 0.000 0.052
#&gt; SRR1576883     2  0.0632      0.909 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1576884     2  0.0632      0.909 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1576885     2  0.0935      0.904 0.000 0.964 0.000 0.004 0.000 0.032
#&gt; SRR1576886     2  0.0935      0.904 0.000 0.964 0.000 0.004 0.000 0.032
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.730           0.890       0.947         0.4657 0.547   0.547
#> 3 3 0.500           0.784       0.769         0.3043 0.799   0.632
#> 4 4 0.617           0.763       0.763         0.1671 0.943   0.836
#> 5 5 0.937           0.935       0.947         0.1356 0.899   0.652
#> 6 6 0.934           0.938       0.945         0.0331 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.000      0.932 0.000 1.000
#&gt; SRR1576834     2   0.000      0.932 0.000 1.000
#&gt; SRR1576835     2   0.000      0.932 0.000 1.000
#&gt; SRR1576836     2   0.000      0.932 0.000 1.000
#&gt; SRR1576837     2   0.839      0.702 0.268 0.732
#&gt; SRR1576838     2   0.839      0.702 0.268 0.732
#&gt; SRR1576839     2   0.839      0.702 0.268 0.732
#&gt; SRR1576840     2   0.839      0.702 0.268 0.732
#&gt; SRR1576841     1   0.808      0.695 0.752 0.248
#&gt; SRR1576842     1   0.808      0.695 0.752 0.248
#&gt; SRR1576843     1   0.808      0.695 0.752 0.248
#&gt; SRR1576844     2   0.000      0.932 0.000 1.000
#&gt; SRR1576845     2   0.000      0.932 0.000 1.000
#&gt; SRR1576846     2   0.000      0.932 0.000 1.000
#&gt; SRR1576847     2   0.000      0.932 0.000 1.000
#&gt; SRR1576848     2   0.000      0.932 0.000 1.000
#&gt; SRR1576849     2   0.000      0.932 0.000 1.000
#&gt; SRR1576850     2   0.000      0.932 0.000 1.000
#&gt; SRR1576851     2   0.000      0.932 0.000 1.000
#&gt; SRR1576852     2   0.000      0.932 0.000 1.000
#&gt; SRR1576853     2   0.839      0.702 0.268 0.732
#&gt; SRR1576854     2   0.839      0.702 0.268 0.732
#&gt; SRR1576855     2   0.839      0.702 0.268 0.732
#&gt; SRR1576856     2   0.839      0.702 0.268 0.732
#&gt; SRR1576857     1   0.000      0.950 1.000 0.000
#&gt; SRR1576858     1   0.000      0.950 1.000 0.000
#&gt; SRR1576859     1   0.000      0.950 1.000 0.000
#&gt; SRR1576860     1   0.000      0.950 1.000 0.000
#&gt; SRR1576861     1   0.000      0.950 1.000 0.000
#&gt; SRR1576862     1   0.000      0.950 1.000 0.000
#&gt; SRR1576863     2   0.000      0.932 0.000 1.000
#&gt; SRR1576864     2   0.000      0.932 0.000 1.000
#&gt; SRR1576865     2   0.000      0.932 0.000 1.000
#&gt; SRR1576866     2   0.000      0.932 0.000 1.000
#&gt; SRR1576867     1   0.000      0.950 1.000 0.000
#&gt; SRR1576868     1   0.000      0.950 1.000 0.000
#&gt; SRR1576869     1   0.000      0.950 1.000 0.000
#&gt; SRR1576870     1   0.000      0.950 1.000 0.000
#&gt; SRR1576871     1   0.000      0.950 1.000 0.000
#&gt; SRR1576872     1   0.000      0.950 1.000 0.000
#&gt; SRR1576873     2   0.000      0.932 0.000 1.000
#&gt; SRR1576874     2   0.000      0.932 0.000 1.000
#&gt; SRR1576875     2   0.000      0.932 0.000 1.000
#&gt; SRR1576876     2   0.000      0.932 0.000 1.000
#&gt; SRR1576877     1   0.000      0.950 1.000 0.000
#&gt; SRR1576878     1   0.000      0.950 1.000 0.000
#&gt; SRR1576879     1   0.000      0.950 1.000 0.000
#&gt; SRR1576880     2   0.000      0.932 0.000 1.000
#&gt; SRR1576881     2   0.000      0.932 0.000 1.000
#&gt; SRR1576882     2   0.000      0.932 0.000 1.000
#&gt; SRR1576883     2   0.000      0.932 0.000 1.000
#&gt; SRR1576884     2   0.000      0.932 0.000 1.000
#&gt; SRR1576885     2   0.000      0.932 0.000 1.000
#&gt; SRR1576886     2   0.000      0.932 0.000 1.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576834     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576835     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576836     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576837     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576838     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576839     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576840     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576841     3   0.949      0.585 0.256 0.248 0.496
#&gt; SRR1576842     3   0.949      0.585 0.256 0.248 0.496
#&gt; SRR1576843     3   0.949      0.585 0.256 0.248 0.496
#&gt; SRR1576844     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576845     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576846     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576847     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576848     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576849     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576850     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576851     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576852     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576853     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576854     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576855     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576856     1   0.960      0.854 0.476 0.256 0.268
#&gt; SRR1576857     3   0.518      0.798 0.256 0.000 0.744
#&gt; SRR1576858     3   0.518      0.798 0.256 0.000 0.744
#&gt; SRR1576859     3   0.518      0.798 0.256 0.000 0.744
#&gt; SRR1576860     3   0.518      0.798 0.256 0.000 0.744
#&gt; SRR1576861     3   0.518      0.798 0.256 0.000 0.744
#&gt; SRR1576862     3   0.518      0.798 0.256 0.000 0.744
#&gt; SRR1576863     1   0.518      0.657 0.744 0.256 0.000
#&gt; SRR1576864     1   0.518      0.657 0.744 0.256 0.000
#&gt; SRR1576865     1   0.518      0.657 0.744 0.256 0.000
#&gt; SRR1576866     1   0.518      0.657 0.744 0.256 0.000
#&gt; SRR1576867     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576868     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576869     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576870     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576871     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576872     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576873     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576874     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576875     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576876     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576877     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576878     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576879     3   0.000      0.822 0.000 0.000 1.000
#&gt; SRR1576880     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576881     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576882     2   0.000      0.803 0.000 1.000 0.000
#&gt; SRR1576883     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576884     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576885     2   0.514      0.774 0.252 0.748 0.000
#&gt; SRR1576886     2   0.514      0.774 0.252 0.748 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576834     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576835     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576836     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576837     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576838     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576839     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576840     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576841     3  0.0895      0.716 0.004 0.000 0.976 0.020
#&gt; SRR1576842     3  0.0895      0.716 0.004 0.000 0.976 0.020
#&gt; SRR1576843     3  0.0895      0.716 0.004 0.000 0.976 0.020
#&gt; SRR1576844     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576845     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576846     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576847     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576848     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576849     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576850     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576851     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576852     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576853     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576854     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576855     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576856     2  0.0000      0.873 0.000 1.000 0.000 0.000
#&gt; SRR1576857     3  0.4220      0.837 0.004 0.248 0.748 0.000
#&gt; SRR1576858     3  0.4220      0.837 0.004 0.248 0.748 0.000
#&gt; SRR1576859     3  0.4220      0.837 0.004 0.248 0.748 0.000
#&gt; SRR1576860     3  0.4220      0.837 0.004 0.248 0.748 0.000
#&gt; SRR1576861     3  0.4220      0.837 0.004 0.248 0.748 0.000
#&gt; SRR1576862     3  0.4220      0.837 0.004 0.248 0.748 0.000
#&gt; SRR1576863     2  0.5394      0.763 0.216 0.732 0.020 0.032
#&gt; SRR1576864     2  0.5394      0.763 0.216 0.732 0.020 0.032
#&gt; SRR1576865     2  0.5394      0.763 0.216 0.732 0.020 0.032
#&gt; SRR1576866     2  0.5394      0.763 0.216 0.732 0.020 0.032
#&gt; SRR1576867     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576868     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576869     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576870     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576871     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576872     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576873     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576874     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576875     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576876     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576877     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576878     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576879     1  0.3764      1.000 0.784 0.216 0.000 0.000
#&gt; SRR1576880     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576881     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576882     4  0.3907      0.663 0.000 0.000 0.232 0.768
#&gt; SRR1576883     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576884     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576885     4  0.4406      0.587 0.000 0.300 0.000 0.700
#&gt; SRR1576886     4  0.4406      0.587 0.000 0.300 0.000 0.700
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1 p2    p3    p4    p5
#&gt; SRR1576833     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576834     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576835     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576836     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576837     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576838     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576839     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576840     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576841     3   0.348      0.709 0.00  0 0.752 0.248 0.000
#&gt; SRR1576842     3   0.348      0.709 0.00  0 0.752 0.248 0.000
#&gt; SRR1576843     3   0.348      0.709 0.00  0 0.752 0.248 0.000
#&gt; SRR1576844     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576845     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576846     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576847     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576848     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576849     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576850     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576851     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576852     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576853     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576854     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576855     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576856     5   0.405      0.888 0.02  0 0.248 0.000 0.732
#&gt; SRR1576857     3   0.000      0.857 0.00  0 1.000 0.000 0.000
#&gt; SRR1576858     3   0.000      0.857 0.00  0 1.000 0.000 0.000
#&gt; SRR1576859     3   0.000      0.857 0.00  0 1.000 0.000 0.000
#&gt; SRR1576860     3   0.000      0.857 0.00  0 1.000 0.000 0.000
#&gt; SRR1576861     3   0.000      0.857 0.00  0 1.000 0.000 0.000
#&gt; SRR1576862     3   0.000      0.857 0.00  0 1.000 0.000 0.000
#&gt; SRR1576863     5   0.000      0.785 0.00  0 0.000 0.000 1.000
#&gt; SRR1576864     5   0.000      0.785 0.00  0 0.000 0.000 1.000
#&gt; SRR1576865     5   0.000      0.785 0.00  0 0.000 0.000 1.000
#&gt; SRR1576866     5   0.000      0.785 0.00  0 0.000 0.000 1.000
#&gt; SRR1576867     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576868     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576869     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576870     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576871     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576872     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576873     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576874     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576875     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576876     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576877     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576878     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576879     1   0.000      1.000 1.00  0 0.000 0.000 0.000
#&gt; SRR1576880     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576881     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576882     4   0.000      1.000 0.00  0 0.000 1.000 0.000
#&gt; SRR1576883     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576884     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576885     2   0.000      1.000 0.00  1 0.000 0.000 0.000
#&gt; SRR1576886     2   0.000      1.000 0.00  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2    p3    p4    p5 p6
#&gt; SRR1576833     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576834     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576835     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576836     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576837     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576838     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576839     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576840     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576841     3   0.000      0.729  0  0 1.000 0.000 0.000  0
#&gt; SRR1576842     3   0.000      0.729  0  0 1.000 0.000 0.000  0
#&gt; SRR1576843     3   0.000      0.729  0  0 1.000 0.000 0.000  0
#&gt; SRR1576844     4   0.313      0.857  0  0 0.248 0.752 0.000  0
#&gt; SRR1576845     4   0.313      0.857  0  0 0.248 0.752 0.000  0
#&gt; SRR1576846     4   0.313      0.857  0  0 0.248 0.752 0.000  0
#&gt; SRR1576847     4   0.000      0.859  0  0 0.000 1.000 0.000  0
#&gt; SRR1576848     4   0.000      0.859  0  0 0.000 1.000 0.000  0
#&gt; SRR1576849     4   0.000      0.859  0  0 0.000 1.000 0.000  0
#&gt; SRR1576850     4   0.000      0.859  0  0 0.000 1.000 0.000  0
#&gt; SRR1576851     4   0.000      0.859  0  0 0.000 1.000 0.000  0
#&gt; SRR1576852     4   0.000      0.859  0  0 0.000 1.000 0.000  0
#&gt; SRR1576853     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576854     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576855     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576856     5   0.000      1.000  0  0 0.000 0.000 1.000  0
#&gt; SRR1576857     3   0.313      0.856  0  0 0.752 0.000 0.248  0
#&gt; SRR1576858     3   0.313      0.856  0  0 0.752 0.000 0.248  0
#&gt; SRR1576859     3   0.313      0.856  0  0 0.752 0.000 0.248  0
#&gt; SRR1576860     3   0.313      0.856  0  0 0.752 0.000 0.248  0
#&gt; SRR1576861     3   0.313      0.856  0  0 0.752 0.000 0.248  0
#&gt; SRR1576862     3   0.313      0.856  0  0 0.752 0.000 0.248  0
#&gt; SRR1576863     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576864     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576865     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576866     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576867     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576868     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576869     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576870     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576871     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576872     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576873     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576874     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576875     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576876     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576877     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576878     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576879     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576880     4   0.310      0.859  0  0 0.244 0.756 0.000  0
#&gt; SRR1576881     4   0.310      0.859  0  0 0.244 0.756 0.000  0
#&gt; SRR1576882     4   0.310      0.859  0  0 0.244 0.756 0.000  0
#&gt; SRR1576883     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576884     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576885     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576886     2   0.000      1.000  0  1 0.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.196           0.816       0.847         0.4663 0.491   0.491
#> 3 3 0.338           0.414       0.612         0.3383 0.798   0.621
#> 4 4 0.403           0.594       0.661         0.1406 0.795   0.502
#> 5 5 0.556           0.642       0.687         0.0805 0.936   0.750
#> 6 6 0.698           0.615       0.698         0.0467 0.962   0.826
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.697      0.853 0.188 0.812
#&gt; SRR1576834     2   0.697      0.853 0.188 0.812
#&gt; SRR1576835     2   0.697      0.853 0.188 0.812
#&gt; SRR1576836     2   0.697      0.853 0.188 0.812
#&gt; SRR1576837     1   0.595      0.828 0.856 0.144
#&gt; SRR1576838     1   0.595      0.828 0.856 0.144
#&gt; SRR1576839     1   0.595      0.828 0.856 0.144
#&gt; SRR1576840     1   0.595      0.828 0.856 0.144
#&gt; SRR1576841     1   0.891      0.697 0.692 0.308
#&gt; SRR1576842     1   0.891      0.697 0.692 0.308
#&gt; SRR1576843     1   0.891      0.697 0.692 0.308
#&gt; SRR1576844     2   0.541      0.809 0.124 0.876
#&gt; SRR1576845     2   0.541      0.809 0.124 0.876
#&gt; SRR1576846     2   0.541      0.809 0.124 0.876
#&gt; SRR1576847     2   0.469      0.826 0.100 0.900
#&gt; SRR1576848     2   0.469      0.826 0.100 0.900
#&gt; SRR1576849     2   0.469      0.826 0.100 0.900
#&gt; SRR1576850     2   0.402      0.822 0.080 0.920
#&gt; SRR1576851     2   0.402      0.822 0.080 0.920
#&gt; SRR1576852     2   0.402      0.822 0.080 0.920
#&gt; SRR1576853     1   0.644      0.816 0.836 0.164
#&gt; SRR1576854     1   0.644      0.816 0.836 0.164
#&gt; SRR1576855     1   0.644      0.816 0.836 0.164
#&gt; SRR1576856     1   0.644      0.816 0.836 0.164
#&gt; SRR1576857     1   0.839      0.763 0.732 0.268
#&gt; SRR1576858     1   0.839      0.763 0.732 0.268
#&gt; SRR1576859     1   0.839      0.763 0.732 0.268
#&gt; SRR1576860     1   0.839      0.763 0.732 0.268
#&gt; SRR1576861     1   0.839      0.763 0.732 0.268
#&gt; SRR1576862     1   0.839      0.763 0.732 0.268
#&gt; SRR1576863     2   0.714      0.818 0.196 0.804
#&gt; SRR1576864     2   0.714      0.818 0.196 0.804
#&gt; SRR1576865     2   0.714      0.818 0.196 0.804
#&gt; SRR1576866     2   0.714      0.818 0.196 0.804
#&gt; SRR1576867     1   0.141      0.841 0.980 0.020
#&gt; SRR1576868     1   0.141      0.841 0.980 0.020
#&gt; SRR1576869     1   0.141      0.841 0.980 0.020
#&gt; SRR1576870     1   0.141      0.841 0.980 0.020
#&gt; SRR1576871     1   0.141      0.841 0.980 0.020
#&gt; SRR1576872     1   0.141      0.841 0.980 0.020
#&gt; SRR1576873     2   0.697      0.853 0.188 0.812
#&gt; SRR1576874     2   0.697      0.853 0.188 0.812
#&gt; SRR1576875     2   0.697      0.853 0.188 0.812
#&gt; SRR1576876     2   0.697      0.853 0.188 0.812
#&gt; SRR1576877     1   0.204      0.836 0.968 0.032
#&gt; SRR1576878     1   0.204      0.836 0.968 0.032
#&gt; SRR1576879     1   0.204      0.836 0.968 0.032
#&gt; SRR1576880     2   0.552      0.807 0.128 0.872
#&gt; SRR1576881     2   0.552      0.807 0.128 0.872
#&gt; SRR1576882     2   0.552      0.807 0.128 0.872
#&gt; SRR1576883     2   0.680      0.851 0.180 0.820
#&gt; SRR1576884     2   0.680      0.851 0.180 0.820
#&gt; SRR1576885     2   0.680      0.851 0.180 0.820
#&gt; SRR1576886     2   0.680      0.851 0.180 0.820
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     3  0.8249    -0.3020 0.076 0.424 0.500
#&gt; SRR1576834     3  0.8249    -0.3020 0.076 0.424 0.500
#&gt; SRR1576835     3  0.8249    -0.3020 0.076 0.424 0.500
#&gt; SRR1576836     3  0.8249    -0.3020 0.076 0.424 0.500
#&gt; SRR1576837     1  0.6380     0.6802 0.732 0.224 0.044
#&gt; SRR1576838     1  0.6380     0.6802 0.732 0.224 0.044
#&gt; SRR1576839     1  0.6380     0.6802 0.732 0.224 0.044
#&gt; SRR1576840     1  0.6380     0.6802 0.732 0.224 0.044
#&gt; SRR1576841     3  0.9446    -0.0391 0.228 0.272 0.500
#&gt; SRR1576842     3  0.9446    -0.0391 0.228 0.272 0.500
#&gt; SRR1576843     3  0.9446    -0.0391 0.228 0.272 0.500
#&gt; SRR1576844     3  0.0592     0.4967 0.012 0.000 0.988
#&gt; SRR1576845     3  0.0592     0.4967 0.012 0.000 0.988
#&gt; SRR1576846     3  0.0592     0.4967 0.012 0.000 0.988
#&gt; SRR1576847     3  0.3845     0.4785 0.012 0.116 0.872
#&gt; SRR1576848     3  0.3845     0.4785 0.012 0.116 0.872
#&gt; SRR1576849     3  0.3845     0.4785 0.012 0.116 0.872
#&gt; SRR1576850     3  0.4692     0.4494 0.012 0.168 0.820
#&gt; SRR1576851     3  0.4692     0.4494 0.012 0.168 0.820
#&gt; SRR1576852     3  0.4692     0.4494 0.012 0.168 0.820
#&gt; SRR1576853     1  0.7228     0.5860 0.600 0.364 0.036
#&gt; SRR1576854     1  0.7228     0.5860 0.600 0.364 0.036
#&gt; SRR1576855     1  0.7228     0.5860 0.600 0.364 0.036
#&gt; SRR1576856     1  0.7228     0.5860 0.600 0.364 0.036
#&gt; SRR1576857     1  0.9566     0.5429 0.440 0.360 0.200
#&gt; SRR1576858     1  0.9566     0.5429 0.440 0.360 0.200
#&gt; SRR1576859     1  0.9566     0.5429 0.440 0.360 0.200
#&gt; SRR1576860     1  0.9566     0.5429 0.440 0.360 0.200
#&gt; SRR1576861     1  0.9566     0.5429 0.440 0.360 0.200
#&gt; SRR1576862     1  0.9566     0.5429 0.440 0.360 0.200
#&gt; SRR1576863     2  0.8645     0.6100 0.148 0.584 0.268
#&gt; SRR1576864     2  0.8645     0.6100 0.148 0.584 0.268
#&gt; SRR1576865     2  0.8645     0.6100 0.148 0.584 0.268
#&gt; SRR1576866     2  0.8645     0.6100 0.148 0.584 0.268
#&gt; SRR1576867     1  0.0424     0.7025 0.992 0.000 0.008
#&gt; SRR1576868     1  0.0424     0.7025 0.992 0.000 0.008
#&gt; SRR1576869     1  0.0424     0.7025 0.992 0.000 0.008
#&gt; SRR1576870     1  0.0424     0.7025 0.992 0.000 0.008
#&gt; SRR1576871     1  0.0424     0.7025 0.992 0.000 0.008
#&gt; SRR1576872     1  0.0424     0.7025 0.992 0.000 0.008
#&gt; SRR1576873     3  0.8236    -0.2830 0.076 0.416 0.508
#&gt; SRR1576874     3  0.8236    -0.2830 0.076 0.416 0.508
#&gt; SRR1576875     3  0.8236    -0.2830 0.076 0.416 0.508
#&gt; SRR1576876     3  0.8236    -0.2830 0.076 0.416 0.508
#&gt; SRR1576877     1  0.2879     0.6785 0.924 0.024 0.052
#&gt; SRR1576878     1  0.2879     0.6785 0.924 0.024 0.052
#&gt; SRR1576879     1  0.2879     0.6785 0.924 0.024 0.052
#&gt; SRR1576880     3  0.0983     0.4961 0.016 0.004 0.980
#&gt; SRR1576881     3  0.0983     0.4961 0.016 0.004 0.980
#&gt; SRR1576882     3  0.0983     0.4961 0.016 0.004 0.980
#&gt; SRR1576883     2  0.8243     0.5092 0.076 0.504 0.420
#&gt; SRR1576884     2  0.8243     0.5092 0.076 0.504 0.420
#&gt; SRR1576885     2  0.8243     0.5092 0.076 0.504 0.420
#&gt; SRR1576886     2  0.8243     0.5092 0.076 0.504 0.420
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.2335      0.917 0.020 0.920 0.000 0.060
#&gt; SRR1576834     2  0.2335      0.917 0.020 0.920 0.000 0.060
#&gt; SRR1576835     2  0.2335      0.917 0.020 0.920 0.000 0.060
#&gt; SRR1576836     2  0.2335      0.917 0.020 0.920 0.000 0.060
#&gt; SRR1576837     1  0.7782      0.474 0.568 0.172 0.224 0.036
#&gt; SRR1576838     1  0.7782      0.474 0.568 0.172 0.224 0.036
#&gt; SRR1576839     1  0.7782      0.474 0.568 0.172 0.224 0.036
#&gt; SRR1576840     1  0.7782      0.474 0.568 0.172 0.224 0.036
#&gt; SRR1576841     4  0.7421      0.184 0.116 0.036 0.256 0.592
#&gt; SRR1576842     4  0.7421      0.184 0.116 0.036 0.256 0.592
#&gt; SRR1576843     4  0.7421      0.184 0.116 0.036 0.256 0.592
#&gt; SRR1576844     4  0.3636      0.729 0.008 0.172 0.000 0.820
#&gt; SRR1576845     4  0.3636      0.729 0.008 0.172 0.000 0.820
#&gt; SRR1576846     4  0.3636      0.729 0.008 0.172 0.000 0.820
#&gt; SRR1576847     4  0.6134      0.686 0.000 0.236 0.104 0.660
#&gt; SRR1576848     4  0.6134      0.686 0.000 0.236 0.104 0.660
#&gt; SRR1576849     4  0.6134      0.686 0.000 0.236 0.104 0.660
#&gt; SRR1576850     4  0.6320      0.667 0.000 0.204 0.140 0.656
#&gt; SRR1576851     4  0.6320      0.667 0.000 0.204 0.140 0.656
#&gt; SRR1576852     4  0.6320      0.667 0.000 0.204 0.140 0.656
#&gt; SRR1576853     1  0.8213      0.367 0.420 0.164 0.384 0.032
#&gt; SRR1576854     1  0.8213      0.367 0.420 0.164 0.384 0.032
#&gt; SRR1576855     1  0.8213      0.367 0.420 0.164 0.384 0.032
#&gt; SRR1576856     1  0.8213      0.367 0.420 0.164 0.384 0.032
#&gt; SRR1576857     3  0.8878      0.473 0.260 0.076 0.456 0.208
#&gt; SRR1576858     3  0.8878      0.473 0.260 0.076 0.456 0.208
#&gt; SRR1576859     3  0.8878      0.473 0.260 0.076 0.456 0.208
#&gt; SRR1576860     3  0.8878      0.473 0.260 0.076 0.456 0.208
#&gt; SRR1576861     3  0.8878      0.473 0.260 0.076 0.456 0.208
#&gt; SRR1576862     3  0.8878      0.473 0.260 0.076 0.456 0.208
#&gt; SRR1576863     3  0.8451      0.164 0.080 0.372 0.440 0.108
#&gt; SRR1576864     3  0.8451      0.164 0.080 0.372 0.440 0.108
#&gt; SRR1576865     3  0.8451      0.164 0.080 0.372 0.440 0.108
#&gt; SRR1576866     3  0.8451      0.164 0.080 0.372 0.440 0.108
#&gt; SRR1576867     1  0.1059      0.642 0.972 0.016 0.000 0.012
#&gt; SRR1576868     1  0.1059      0.642 0.972 0.016 0.000 0.012
#&gt; SRR1576869     1  0.1059      0.642 0.972 0.016 0.000 0.012
#&gt; SRR1576870     1  0.0937      0.641 0.976 0.012 0.000 0.012
#&gt; SRR1576871     1  0.0937      0.641 0.976 0.012 0.000 0.012
#&gt; SRR1576872     1  0.0937      0.641 0.976 0.012 0.000 0.012
#&gt; SRR1576873     2  0.2441      0.917 0.020 0.920 0.004 0.056
#&gt; SRR1576874     2  0.2441      0.917 0.020 0.920 0.004 0.056
#&gt; SRR1576875     2  0.2441      0.917 0.020 0.920 0.004 0.056
#&gt; SRR1576876     2  0.2441      0.917 0.020 0.920 0.004 0.056
#&gt; SRR1576877     1  0.4976      0.577 0.800 0.020 0.084 0.096
#&gt; SRR1576878     1  0.4976      0.577 0.800 0.020 0.084 0.096
#&gt; SRR1576879     1  0.4976      0.577 0.800 0.020 0.084 0.096
#&gt; SRR1576880     4  0.3823      0.723 0.008 0.160 0.008 0.824
#&gt; SRR1576881     4  0.3823      0.723 0.008 0.160 0.008 0.824
#&gt; SRR1576882     4  0.3823      0.723 0.008 0.160 0.008 0.824
#&gt; SRR1576883     2  0.3943      0.832 0.024 0.856 0.088 0.032
#&gt; SRR1576884     2  0.3943      0.832 0.024 0.856 0.088 0.032
#&gt; SRR1576885     2  0.3943      0.832 0.024 0.856 0.088 0.032
#&gt; SRR1576886     2  0.3943      0.832 0.024 0.856 0.088 0.032
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.1554     0.8677 0.012 0.952 0.024 0.008 0.004
#&gt; SRR1576834     2  0.1554     0.8677 0.012 0.952 0.024 0.008 0.004
#&gt; SRR1576835     2  0.1554     0.8677 0.012 0.952 0.024 0.008 0.004
#&gt; SRR1576836     2  0.1554     0.8677 0.012 0.952 0.024 0.008 0.004
#&gt; SRR1576837     1  0.8567     0.0671 0.424 0.148 0.248 0.028 0.152
#&gt; SRR1576838     1  0.8567     0.0671 0.424 0.148 0.248 0.028 0.152
#&gt; SRR1576839     1  0.8567     0.0671 0.424 0.148 0.248 0.028 0.152
#&gt; SRR1576840     1  0.8567     0.0671 0.424 0.148 0.248 0.028 0.152
#&gt; SRR1576841     4  0.6847     0.0680 0.044 0.016 0.376 0.496 0.068
#&gt; SRR1576842     4  0.6847     0.0680 0.044 0.016 0.376 0.496 0.068
#&gt; SRR1576843     4  0.6847     0.0680 0.044 0.016 0.376 0.496 0.068
#&gt; SRR1576844     4  0.2464     0.7006 0.000 0.096 0.016 0.888 0.000
#&gt; SRR1576845     4  0.2464     0.7006 0.000 0.096 0.016 0.888 0.000
#&gt; SRR1576846     4  0.2464     0.7006 0.000 0.096 0.016 0.888 0.000
#&gt; SRR1576847     4  0.7158     0.6612 0.012 0.148 0.096 0.596 0.148
#&gt; SRR1576848     4  0.7158     0.6612 0.012 0.148 0.096 0.596 0.148
#&gt; SRR1576849     4  0.7158     0.6612 0.012 0.148 0.096 0.596 0.148
#&gt; SRR1576850     4  0.7139     0.6382 0.012 0.100 0.096 0.584 0.208
#&gt; SRR1576851     4  0.7139     0.6382 0.012 0.100 0.096 0.584 0.208
#&gt; SRR1576852     4  0.7139     0.6382 0.012 0.100 0.096 0.584 0.208
#&gt; SRR1576853     5  0.8297     0.4679 0.296 0.100 0.204 0.012 0.388
#&gt; SRR1576854     5  0.8297     0.4679 0.296 0.100 0.204 0.012 0.388
#&gt; SRR1576855     5  0.8297     0.4679 0.296 0.100 0.204 0.012 0.388
#&gt; SRR1576856     5  0.8297     0.4679 0.296 0.100 0.204 0.012 0.388
#&gt; SRR1576857     3  0.5352     0.9918 0.176 0.048 0.724 0.044 0.008
#&gt; SRR1576858     3  0.5352     0.9918 0.176 0.048 0.724 0.044 0.008
#&gt; SRR1576859     3  0.5352     0.9918 0.176 0.048 0.724 0.044 0.008
#&gt; SRR1576860     3  0.4971     0.9918 0.172 0.048 0.740 0.040 0.000
#&gt; SRR1576861     3  0.4971     0.9918 0.172 0.048 0.740 0.040 0.000
#&gt; SRR1576862     3  0.4971     0.9918 0.172 0.048 0.740 0.040 0.000
#&gt; SRR1576863     5  0.6466     0.5877 0.044 0.200 0.068 0.036 0.652
#&gt; SRR1576864     5  0.6466     0.5877 0.044 0.200 0.068 0.036 0.652
#&gt; SRR1576865     5  0.6466     0.5877 0.044 0.200 0.068 0.036 0.652
#&gt; SRR1576866     5  0.6466     0.5877 0.044 0.200 0.068 0.036 0.652
#&gt; SRR1576867     1  0.0671     0.6942 0.980 0.016 0.000 0.004 0.000
#&gt; SRR1576868     1  0.0671     0.6942 0.980 0.016 0.000 0.004 0.000
#&gt; SRR1576869     1  0.0671     0.6942 0.980 0.016 0.000 0.004 0.000
#&gt; SRR1576870     1  0.0510     0.6942 0.984 0.016 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0510     0.6942 0.984 0.016 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0510     0.6942 0.984 0.016 0.000 0.000 0.000
#&gt; SRR1576873     2  0.1602     0.8651 0.012 0.952 0.016 0.012 0.008
#&gt; SRR1576874     2  0.1602     0.8651 0.012 0.952 0.016 0.012 0.008
#&gt; SRR1576875     2  0.1602     0.8651 0.012 0.952 0.016 0.012 0.008
#&gt; SRR1576876     2  0.1602     0.8651 0.012 0.952 0.016 0.012 0.008
#&gt; SRR1576877     1  0.4895     0.6256 0.788 0.024 0.036 0.068 0.084
#&gt; SRR1576878     1  0.4895     0.6256 0.788 0.024 0.036 0.068 0.084
#&gt; SRR1576879     1  0.4895     0.6256 0.788 0.024 0.036 0.068 0.084
#&gt; SRR1576880     4  0.2954     0.6961 0.008 0.080 0.028 0.880 0.004
#&gt; SRR1576881     4  0.2954     0.6961 0.008 0.080 0.028 0.880 0.004
#&gt; SRR1576882     4  0.2954     0.6961 0.008 0.080 0.028 0.880 0.004
#&gt; SRR1576883     2  0.5007     0.7446 0.000 0.736 0.048 0.040 0.176
#&gt; SRR1576884     2  0.5007     0.7446 0.000 0.736 0.048 0.040 0.176
#&gt; SRR1576885     2  0.5007     0.7446 0.000 0.736 0.048 0.040 0.176
#&gt; SRR1576886     2  0.5007     0.7446 0.000 0.736 0.048 0.040 0.176
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1576833     2   0.118     0.8672 0.000 0.960 0.004 0.024 0.004 NA
#&gt; SRR1576834     2   0.118     0.8672 0.000 0.960 0.004 0.024 0.004 NA
#&gt; SRR1576835     2   0.118     0.8672 0.000 0.960 0.004 0.024 0.008 NA
#&gt; SRR1576836     2   0.118     0.8672 0.000 0.960 0.004 0.024 0.008 NA
#&gt; SRR1576837     1   0.897    -0.0998 0.260 0.112 0.244 0.008 0.188 NA
#&gt; SRR1576838     1   0.897    -0.0998 0.260 0.112 0.244 0.008 0.188 NA
#&gt; SRR1576839     1   0.897    -0.0998 0.260 0.112 0.244 0.008 0.188 NA
#&gt; SRR1576840     1   0.897    -0.0998 0.260 0.112 0.244 0.008 0.188 NA
#&gt; SRR1576841     3   0.734     0.3088 0.044 0.004 0.360 0.244 0.020 NA
#&gt; SRR1576842     3   0.734     0.3088 0.044 0.004 0.360 0.244 0.020 NA
#&gt; SRR1576843     3   0.734     0.3088 0.044 0.004 0.360 0.244 0.020 NA
#&gt; SRR1576844     4   0.460     0.6879 0.000 0.020 0.008 0.612 0.008 NA
#&gt; SRR1576845     4   0.460     0.6879 0.000 0.020 0.008 0.612 0.008 NA
#&gt; SRR1576846     4   0.460     0.6879 0.000 0.020 0.008 0.612 0.008 NA
#&gt; SRR1576847     4   0.233     0.7025 0.000 0.060 0.040 0.896 0.004 NA
#&gt; SRR1576848     4   0.233     0.7025 0.000 0.060 0.040 0.896 0.004 NA
#&gt; SRR1576849     4   0.233     0.7025 0.000 0.060 0.040 0.896 0.004 NA
#&gt; SRR1576850     4   0.331     0.6913 0.000 0.048 0.052 0.848 0.052 NA
#&gt; SRR1576851     4   0.331     0.6913 0.000 0.048 0.052 0.848 0.052 NA
#&gt; SRR1576852     4   0.331     0.6913 0.000 0.048 0.052 0.848 0.052 NA
#&gt; SRR1576853     5   0.805     0.5008 0.188 0.064 0.172 0.000 0.424 NA
#&gt; SRR1576854     5   0.805     0.5008 0.188 0.064 0.172 0.000 0.424 NA
#&gt; SRR1576855     5   0.805     0.5008 0.188 0.064 0.172 0.000 0.424 NA
#&gt; SRR1576856     5   0.805     0.5008 0.188 0.064 0.172 0.000 0.424 NA
#&gt; SRR1576857     3   0.373     0.7242 0.084 0.024 0.836 0.024 0.016 NA
#&gt; SRR1576858     3   0.373     0.7242 0.084 0.024 0.836 0.024 0.016 NA
#&gt; SRR1576859     3   0.373     0.7242 0.084 0.024 0.836 0.024 0.016 NA
#&gt; SRR1576860     3   0.271     0.7242 0.080 0.024 0.876 0.020 0.000 NA
#&gt; SRR1576861     3   0.271     0.7242 0.080 0.024 0.876 0.020 0.000 NA
#&gt; SRR1576862     3   0.271     0.7242 0.080 0.024 0.876 0.020 0.000 NA
#&gt; SRR1576863     5   0.436     0.6241 0.016 0.088 0.036 0.056 0.796 NA
#&gt; SRR1576864     5   0.436     0.6241 0.016 0.088 0.036 0.056 0.796 NA
#&gt; SRR1576865     5   0.402     0.6241 0.012 0.088 0.036 0.056 0.808 NA
#&gt; SRR1576866     5   0.402     0.6241 0.012 0.088 0.036 0.056 0.808 NA
#&gt; SRR1576867     1   0.206     0.6479 0.924 0.024 0.028 0.000 0.012 NA
#&gt; SRR1576868     1   0.206     0.6479 0.924 0.024 0.028 0.000 0.012 NA
#&gt; SRR1576869     1   0.206     0.6479 0.924 0.024 0.028 0.000 0.012 NA
#&gt; SRR1576870     1   0.140     0.6479 0.948 0.024 0.024 0.000 0.004 NA
#&gt; SRR1576871     1   0.140     0.6479 0.948 0.024 0.024 0.000 0.004 NA
#&gt; SRR1576872     1   0.140     0.6479 0.948 0.024 0.024 0.000 0.004 NA
#&gt; SRR1576873     2   0.298     0.8557 0.000 0.872 0.036 0.036 0.004 NA
#&gt; SRR1576874     2   0.298     0.8557 0.000 0.872 0.036 0.036 0.004 NA
#&gt; SRR1576875     2   0.298     0.8557 0.000 0.872 0.036 0.036 0.004 NA
#&gt; SRR1576876     2   0.298     0.8557 0.000 0.872 0.036 0.036 0.004 NA
#&gt; SRR1576877     1   0.489     0.5667 0.720 0.024 0.008 0.016 0.040 NA
#&gt; SRR1576878     1   0.486     0.5667 0.720 0.024 0.008 0.016 0.036 NA
#&gt; SRR1576879     1   0.489     0.5667 0.720 0.024 0.008 0.016 0.040 NA
#&gt; SRR1576880     4   0.480     0.6790 0.012 0.016 0.008 0.588 0.004 NA
#&gt; SRR1576881     4   0.480     0.6790 0.012 0.016 0.008 0.588 0.004 NA
#&gt; SRR1576882     4   0.480     0.6790 0.012 0.016 0.008 0.588 0.004 NA
#&gt; SRR1576883     2   0.440     0.7757 0.000 0.752 0.012 0.004 0.124 NA
#&gt; SRR1576884     2   0.440     0.7757 0.000 0.752 0.012 0.004 0.124 NA
#&gt; SRR1576885     2   0.435     0.7757 0.000 0.752 0.008 0.004 0.124 NA
#&gt; SRR1576886     2   0.435     0.7757 0.000 0.752 0.008 0.004 0.124 NA
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.998       0.999          0.509 0.491   0.491
#> 3 3 0.807           0.916       0.936          0.303 0.792   0.598
#> 4 4 0.762           0.865       0.892          0.129 0.891   0.685
#> 5 5 0.857           0.915       0.925          0.079 0.894   0.610
#> 6 6 0.869           0.818       0.824          0.033 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576834     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576835     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576836     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576837     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576838     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576839     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576840     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576841     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576842     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576843     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576844     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576845     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576846     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576847     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576848     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576849     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576850     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576851     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576852     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576853     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576854     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576855     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576856     1  0.0376      0.997 0.996 0.004
#&gt; SRR1576857     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576858     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576859     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576860     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576861     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576862     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576863     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576864     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576865     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576866     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576867     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576868     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576869     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576870     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576871     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576872     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576873     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576874     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576875     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576876     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576877     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576878     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576879     1  0.0000      0.999 1.000 0.000
#&gt; SRR1576880     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576881     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576882     2  0.0376      0.998 0.004 0.996
#&gt; SRR1576883     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576884     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576885     2  0.0000      0.998 0.000 1.000
#&gt; SRR1576886     2  0.0000      0.998 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576834     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576835     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576836     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576837     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576838     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576839     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576840     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576841     3  0.1529      0.880 0.040 0.000 0.960
#&gt; SRR1576842     3  0.1529      0.880 0.040 0.000 0.960
#&gt; SRR1576843     3  0.1529      0.880 0.040 0.000 0.960
#&gt; SRR1576844     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576845     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576846     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576847     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576848     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576849     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576850     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576851     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576852     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576853     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576854     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576855     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576856     1  0.1170      0.903 0.976 0.016 0.008
#&gt; SRR1576857     1  0.5706      0.673 0.680 0.000 0.320
#&gt; SRR1576858     1  0.5706      0.673 0.680 0.000 0.320
#&gt; SRR1576859     1  0.5706      0.673 0.680 0.000 0.320
#&gt; SRR1576860     1  0.5706      0.673 0.680 0.000 0.320
#&gt; SRR1576861     1  0.5706      0.673 0.680 0.000 0.320
#&gt; SRR1576862     1  0.5706      0.673 0.680 0.000 0.320
#&gt; SRR1576863     2  0.1031      0.973 0.000 0.976 0.024
#&gt; SRR1576864     2  0.1031      0.973 0.000 0.976 0.024
#&gt; SRR1576865     2  0.1031      0.973 0.000 0.976 0.024
#&gt; SRR1576866     2  0.1031      0.973 0.000 0.976 0.024
#&gt; SRR1576867     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576873     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576874     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576875     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576876     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576877     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576880     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576881     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576882     3  0.2537      0.970 0.000 0.080 0.920
#&gt; SRR1576883     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576884     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576885     2  0.0747      0.991 0.000 0.984 0.016
#&gt; SRR1576886     2  0.0747      0.991 0.000 0.984 0.016
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576834     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576835     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576836     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576837     1  0.5055      0.791 0.712 0.032 0.256 0.000
#&gt; SRR1576838     1  0.5055      0.791 0.712 0.032 0.256 0.000
#&gt; SRR1576839     1  0.5085      0.789 0.708 0.032 0.260 0.000
#&gt; SRR1576840     1  0.5085      0.789 0.708 0.032 0.260 0.000
#&gt; SRR1576841     3  0.5763      0.796 0.096 0.000 0.700 0.204
#&gt; SRR1576842     3  0.5763      0.796 0.096 0.000 0.700 0.204
#&gt; SRR1576843     3  0.5763      0.796 0.096 0.000 0.700 0.204
#&gt; SRR1576844     4  0.0657      0.986 0.000 0.004 0.012 0.984
#&gt; SRR1576845     4  0.0657      0.986 0.000 0.004 0.012 0.984
#&gt; SRR1576846     4  0.0657      0.986 0.000 0.004 0.012 0.984
#&gt; SRR1576847     4  0.0336      0.987 0.000 0.000 0.008 0.992
#&gt; SRR1576848     4  0.0336      0.987 0.000 0.000 0.008 0.992
#&gt; SRR1576849     4  0.0336      0.987 0.000 0.000 0.008 0.992
#&gt; SRR1576850     4  0.0336      0.987 0.000 0.000 0.008 0.992
#&gt; SRR1576851     4  0.0336      0.987 0.000 0.000 0.008 0.992
#&gt; SRR1576852     4  0.0336      0.987 0.000 0.000 0.008 0.992
#&gt; SRR1576853     1  0.5599      0.760 0.644 0.040 0.316 0.000
#&gt; SRR1576854     1  0.5599      0.760 0.644 0.040 0.316 0.000
#&gt; SRR1576855     1  0.5599      0.760 0.644 0.040 0.316 0.000
#&gt; SRR1576856     1  0.5599      0.760 0.644 0.040 0.316 0.000
#&gt; SRR1576857     3  0.4175      0.899 0.200 0.000 0.784 0.016
#&gt; SRR1576858     3  0.4175      0.899 0.200 0.000 0.784 0.016
#&gt; SRR1576859     3  0.4175      0.899 0.200 0.000 0.784 0.016
#&gt; SRR1576860     3  0.4175      0.899 0.200 0.000 0.784 0.016
#&gt; SRR1576861     3  0.4175      0.899 0.200 0.000 0.784 0.016
#&gt; SRR1576862     3  0.4175      0.899 0.200 0.000 0.784 0.016
#&gt; SRR1576863     2  0.6330      0.684 0.008 0.672 0.208 0.112
#&gt; SRR1576864     2  0.6330      0.684 0.008 0.672 0.208 0.112
#&gt; SRR1576865     2  0.6330      0.684 0.008 0.672 0.208 0.112
#&gt; SRR1576866     2  0.6330      0.684 0.008 0.672 0.208 0.112
#&gt; SRR1576867     1  0.0000      0.813 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.813 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.813 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.813 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.813 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.813 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576874     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576875     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576876     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576877     1  0.0927      0.800 0.976 0.000 0.016 0.008
#&gt; SRR1576878     1  0.0927      0.800 0.976 0.000 0.016 0.008
#&gt; SRR1576879     1  0.0927      0.800 0.976 0.000 0.016 0.008
#&gt; SRR1576880     4  0.0657      0.986 0.000 0.004 0.012 0.984
#&gt; SRR1576881     4  0.0657      0.986 0.000 0.004 0.012 0.984
#&gt; SRR1576882     4  0.0657      0.986 0.000 0.004 0.012 0.984
#&gt; SRR1576883     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576884     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576885     2  0.1211      0.907 0.000 0.960 0.000 0.040
#&gt; SRR1576886     2  0.1211      0.907 0.000 0.960 0.000 0.040
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.5525      0.676 0.288 0.000 0.100 0.000 0.612
#&gt; SRR1576838     5  0.5525      0.676 0.288 0.000 0.100 0.000 0.612
#&gt; SRR1576839     5  0.5525      0.676 0.288 0.000 0.100 0.000 0.612
#&gt; SRR1576840     5  0.5525      0.676 0.288 0.000 0.100 0.000 0.612
#&gt; SRR1576841     3  0.2660      0.878 0.008 0.000 0.864 0.128 0.000
#&gt; SRR1576842     3  0.2660      0.878 0.008 0.000 0.864 0.128 0.000
#&gt; SRR1576843     3  0.2660      0.878 0.008 0.000 0.864 0.128 0.000
#&gt; SRR1576844     4  0.0703      0.935 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1576845     4  0.0703      0.935 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1576846     4  0.0703      0.935 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1576847     4  0.1908      0.936 0.000 0.000 0.000 0.908 0.092
#&gt; SRR1576848     4  0.1908      0.936 0.000 0.000 0.000 0.908 0.092
#&gt; SRR1576849     4  0.1908      0.936 0.000 0.000 0.000 0.908 0.092
#&gt; SRR1576850     4  0.1908      0.936 0.000 0.000 0.000 0.908 0.092
#&gt; SRR1576851     4  0.1908      0.936 0.000 0.000 0.000 0.908 0.092
#&gt; SRR1576852     4  0.1908      0.936 0.000 0.000 0.000 0.908 0.092
#&gt; SRR1576853     5  0.2659      0.825 0.060 0.000 0.052 0.000 0.888
#&gt; SRR1576854     5  0.2659      0.825 0.060 0.000 0.052 0.000 0.888
#&gt; SRR1576855     5  0.2659      0.825 0.060 0.000 0.052 0.000 0.888
#&gt; SRR1576856     5  0.2659      0.825 0.060 0.000 0.052 0.000 0.888
#&gt; SRR1576857     3  0.0693      0.939 0.012 0.000 0.980 0.000 0.008
#&gt; SRR1576858     3  0.0693      0.939 0.012 0.000 0.980 0.000 0.008
#&gt; SRR1576859     3  0.0693      0.939 0.012 0.000 0.980 0.000 0.008
#&gt; SRR1576860     3  0.0693      0.939 0.012 0.000 0.980 0.000 0.008
#&gt; SRR1576861     3  0.0693      0.939 0.012 0.000 0.980 0.000 0.008
#&gt; SRR1576862     3  0.0693      0.939 0.012 0.000 0.980 0.000 0.008
#&gt; SRR1576863     5  0.1331      0.793 0.000 0.040 0.000 0.008 0.952
#&gt; SRR1576864     5  0.1331      0.793 0.000 0.040 0.000 0.008 0.952
#&gt; SRR1576865     5  0.1331      0.793 0.000 0.040 0.000 0.008 0.952
#&gt; SRR1576866     5  0.1331      0.793 0.000 0.040 0.000 0.008 0.952
#&gt; SRR1576867     1  0.0290      0.985 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1576868     1  0.0290      0.985 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1576869     1  0.0290      0.985 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1576870     1  0.0290      0.985 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1576871     1  0.0290      0.985 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1576872     1  0.0290      0.985 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1576873     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0798      0.970 0.976 0.000 0.008 0.016 0.000
#&gt; SRR1576878     1  0.0798      0.970 0.976 0.000 0.008 0.016 0.000
#&gt; SRR1576879     1  0.0798      0.970 0.976 0.000 0.008 0.016 0.000
#&gt; SRR1576880     4  0.0703      0.935 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1576881     4  0.0703      0.935 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1576882     4  0.0703      0.935 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1576883     2  0.0510      0.989 0.000 0.984 0.000 0.000 0.016
#&gt; SRR1576884     2  0.0510      0.989 0.000 0.984 0.000 0.000 0.016
#&gt; SRR1576885     2  0.0510      0.989 0.000 0.984 0.000 0.000 0.016
#&gt; SRR1576886     2  0.0510      0.989 0.000 0.984 0.000 0.000 0.016
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.6251      1.000 0.156 0.000 0.056 0.000 0.552 0.236
#&gt; SRR1576838     5  0.6251      1.000 0.156 0.000 0.056 0.000 0.552 0.236
#&gt; SRR1576839     5  0.6251      1.000 0.156 0.000 0.056 0.000 0.552 0.236
#&gt; SRR1576840     5  0.6251      1.000 0.156 0.000 0.056 0.000 0.552 0.236
#&gt; SRR1576841     3  0.4709      0.637 0.004 0.000 0.596 0.048 0.352 0.000
#&gt; SRR1576842     3  0.4709      0.637 0.004 0.000 0.596 0.048 0.352 0.000
#&gt; SRR1576843     3  0.4709      0.637 0.004 0.000 0.596 0.048 0.352 0.000
#&gt; SRR1576844     4  0.3620      0.760 0.000 0.000 0.000 0.648 0.352 0.000
#&gt; SRR1576845     4  0.3620      0.760 0.000 0.000 0.000 0.648 0.352 0.000
#&gt; SRR1576846     4  0.3620      0.760 0.000 0.000 0.000 0.648 0.352 0.000
#&gt; SRR1576847     4  0.1075      0.779 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1576848     4  0.1075      0.779 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1576849     4  0.1075      0.779 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1576850     4  0.1075      0.779 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1576851     4  0.1075      0.779 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1576852     4  0.1075      0.779 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1576853     6  0.4254      0.311 0.020 0.000 0.000 0.000 0.404 0.576
#&gt; SRR1576854     6  0.4254      0.311 0.020 0.000 0.000 0.000 0.404 0.576
#&gt; SRR1576855     6  0.4254      0.311 0.020 0.000 0.000 0.000 0.404 0.576
#&gt; SRR1576856     6  0.4254      0.311 0.020 0.000 0.000 0.000 0.404 0.576
#&gt; SRR1576857     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     6  0.0260      0.617 0.000 0.008 0.000 0.000 0.000 0.992
#&gt; SRR1576864     6  0.0260      0.617 0.000 0.008 0.000 0.000 0.000 0.992
#&gt; SRR1576865     6  0.0260      0.617 0.000 0.008 0.000 0.000 0.000 0.992
#&gt; SRR1576866     6  0.0260      0.617 0.000 0.008 0.000 0.000 0.000 0.992
#&gt; SRR1576867     1  0.0146      0.983 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0146      0.983 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0146      0.983 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0146      0.983 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0146      0.983 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0146      0.983 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0937      0.966 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1576878     1  0.0937      0.966 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1576879     1  0.0937      0.966 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1576880     4  0.3607      0.762 0.000 0.000 0.000 0.652 0.348 0.000
#&gt; SRR1576881     4  0.3607      0.762 0.000 0.000 0.000 0.652 0.348 0.000
#&gt; SRR1576882     4  0.3607      0.762 0.000 0.000 0.000 0.652 0.348 0.000
#&gt; SRR1576883     2  0.2164      0.932 0.000 0.900 0.000 0.000 0.032 0.068
#&gt; SRR1576884     2  0.2164      0.932 0.000 0.900 0.000 0.000 0.032 0.068
#&gt; SRR1576885     2  0.2164      0.932 0.000 0.900 0.000 0.000 0.032 0.068
#&gt; SRR1576886     2  0.2164      0.932 0.000 0.900 0.000 0.000 0.032 0.068
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.973       0.983          0.365 0.648   0.648
#> 3 3 0.644           0.937       0.936          0.660 0.748   0.612
#> 4 4 0.818           0.940       0.950          0.216 0.868   0.667
#> 5 5 1.000           1.000       1.000          0.101 0.925   0.714
#> 6 6 0.945           0.962       0.955          0.027 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576834     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576835     1  0.5629      0.882 0.868 0.132
#&gt; SRR1576836     1  0.4161      0.932 0.916 0.084
#&gt; SRR1576837     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576838     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576839     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576840     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576841     1  0.0376      0.974 0.996 0.004
#&gt; SRR1576842     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576843     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576844     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576845     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576846     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576847     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576848     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576849     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576850     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576851     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576852     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576853     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576854     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576855     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576856     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576857     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576858     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576859     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576860     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576861     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576862     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576863     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576864     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576865     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576866     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576867     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576868     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576869     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576870     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576871     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576872     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576873     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576874     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576875     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576876     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576877     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576878     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576879     1  0.0000      0.977 1.000 0.000
#&gt; SRR1576880     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576881     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576882     2  0.0000      1.000 0.000 1.000
#&gt; SRR1576883     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576884     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576885     1  0.3733      0.941 0.928 0.072
#&gt; SRR1576886     1  0.3733      0.941 0.928 0.072
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576834     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576835     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576836     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576837     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576838     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576839     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576840     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576841     1  0.5276      0.845 0.820 0.128 0.052
#&gt; SRR1576842     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576843     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576844     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576845     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576846     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576847     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576848     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576849     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576850     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576851     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576852     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576853     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576854     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576855     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576856     1  0.0592      0.923 0.988 0.012 0.000
#&gt; SRR1576857     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576858     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576859     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576860     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576861     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576862     1  0.3482      0.882 0.872 0.128 0.000
#&gt; SRR1576863     1  0.4399      0.748 0.812 0.188 0.000
#&gt; SRR1576864     1  0.4399      0.748 0.812 0.188 0.000
#&gt; SRR1576865     1  0.4399      0.748 0.812 0.188 0.000
#&gt; SRR1576866     1  0.4399      0.748 0.812 0.188 0.000
#&gt; SRR1576867     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576873     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576874     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576875     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576876     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576877     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.924 1.000 0.000 0.000
#&gt; SRR1576880     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576881     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576882     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1576883     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576884     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576885     2  0.3267      1.000 0.116 0.884 0.000
#&gt; SRR1576886     2  0.3267      1.000 0.116 0.884 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4
#&gt; SRR1576833     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576834     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576835     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576836     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576837     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576838     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576839     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576840     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576841     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576842     3  0.0707      0.976 0.020 0.000 0.980  0
#&gt; SRR1576843     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576844     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576845     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576846     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576847     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576848     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576849     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576850     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576851     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576852     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576853     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576854     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576855     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576856     1  0.3948      0.874 0.828 0.036 0.136  0
#&gt; SRR1576857     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576858     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576859     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576860     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576861     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576862     3  0.0000      0.997 0.000 0.000 1.000  0
#&gt; SRR1576863     1  0.5628      0.780 0.704 0.216 0.080  0
#&gt; SRR1576864     1  0.5628      0.780 0.704 0.216 0.080  0
#&gt; SRR1576865     1  0.5628      0.780 0.704 0.216 0.080  0
#&gt; SRR1576866     1  0.5628      0.780 0.704 0.216 0.080  0
#&gt; SRR1576867     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576868     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576869     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576870     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576871     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576872     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576873     2  0.1059      0.976 0.012 0.972 0.016  0
#&gt; SRR1576874     2  0.1059      0.976 0.012 0.972 0.016  0
#&gt; SRR1576875     2  0.1059      0.976 0.012 0.972 0.016  0
#&gt; SRR1576876     2  0.1059      0.976 0.012 0.972 0.016  0
#&gt; SRR1576877     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576878     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576879     1  0.0000      0.874 1.000 0.000 0.000  0
#&gt; SRR1576880     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576881     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576882     4  0.0000      1.000 0.000 0.000 0.000  1
#&gt; SRR1576883     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576884     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576885     2  0.0000      0.988 0.000 1.000 0.000  0
#&gt; SRR1576886     2  0.0000      0.988 0.000 1.000 0.000  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5
#&gt; SRR1576833     2       0          1  0  1  0  0  0
#&gt; SRR1576834     2       0          1  0  1  0  0  0
#&gt; SRR1576835     2       0          1  0  1  0  0  0
#&gt; SRR1576836     2       0          1  0  1  0  0  0
#&gt; SRR1576837     5       0          1  0  0  0  0  1
#&gt; SRR1576838     5       0          1  0  0  0  0  1
#&gt; SRR1576839     5       0          1  0  0  0  0  1
#&gt; SRR1576840     5       0          1  0  0  0  0  1
#&gt; SRR1576841     3       0          1  0  0  1  0  0
#&gt; SRR1576842     3       0          1  0  0  1  0  0
#&gt; SRR1576843     3       0          1  0  0  1  0  0
#&gt; SRR1576844     4       0          1  0  0  0  1  0
#&gt; SRR1576845     4       0          1  0  0  0  1  0
#&gt; SRR1576846     4       0          1  0  0  0  1  0
#&gt; SRR1576847     4       0          1  0  0  0  1  0
#&gt; SRR1576848     4       0          1  0  0  0  1  0
#&gt; SRR1576849     4       0          1  0  0  0  1  0
#&gt; SRR1576850     4       0          1  0  0  0  1  0
#&gt; SRR1576851     4       0          1  0  0  0  1  0
#&gt; SRR1576852     4       0          1  0  0  0  1  0
#&gt; SRR1576853     5       0          1  0  0  0  0  1
#&gt; SRR1576854     5       0          1  0  0  0  0  1
#&gt; SRR1576855     5       0          1  0  0  0  0  1
#&gt; SRR1576856     5       0          1  0  0  0  0  1
#&gt; SRR1576857     3       0          1  0  0  1  0  0
#&gt; SRR1576858     3       0          1  0  0  1  0  0
#&gt; SRR1576859     3       0          1  0  0  1  0  0
#&gt; SRR1576860     3       0          1  0  0  1  0  0
#&gt; SRR1576861     3       0          1  0  0  1  0  0
#&gt; SRR1576862     3       0          1  0  0  1  0  0
#&gt; SRR1576863     5       0          1  0  0  0  0  1
#&gt; SRR1576864     5       0          1  0  0  0  0  1
#&gt; SRR1576865     5       0          1  0  0  0  0  1
#&gt; SRR1576866     5       0          1  0  0  0  0  1
#&gt; SRR1576867     1       0          1  1  0  0  0  0
#&gt; SRR1576868     1       0          1  1  0  0  0  0
#&gt; SRR1576869     1       0          1  1  0  0  0  0
#&gt; SRR1576870     1       0          1  1  0  0  0  0
#&gt; SRR1576871     1       0          1  1  0  0  0  0
#&gt; SRR1576872     1       0          1  1  0  0  0  0
#&gt; SRR1576873     2       0          1  0  1  0  0  0
#&gt; SRR1576874     2       0          1  0  1  0  0  0
#&gt; SRR1576875     2       0          1  0  1  0  0  0
#&gt; SRR1576876     2       0          1  0  1  0  0  0
#&gt; SRR1576877     1       0          1  1  0  0  0  0
#&gt; SRR1576878     1       0          1  1  0  0  0  0
#&gt; SRR1576879     1       0          1  1  0  0  0  0
#&gt; SRR1576880     4       0          1  0  0  0  1  0
#&gt; SRR1576881     4       0          1  0  0  0  1  0
#&gt; SRR1576882     4       0          1  0  0  0  1  0
#&gt; SRR1576883     2       0          1  0  1  0  0  0
#&gt; SRR1576884     2       0          1  0  1  0  0  0
#&gt; SRR1576885     2       0          1  0  1  0  0  0
#&gt; SRR1576886     2       0          1  0  1  0  0  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3 p4    p5   p6
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576837     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576838     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576839     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576840     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576841     3  0.0713      0.976 0.000 0.00 0.972  0 0.028 0.00
#&gt; SRR1576842     3  0.0713      0.976 0.000 0.00 0.972  0 0.028 0.00
#&gt; SRR1576843     3  0.0713      0.976 0.000 0.00 0.972  0 0.028 0.00
#&gt; SRR1576844     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576845     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576846     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576847     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576848     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576849     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576850     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576851     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576852     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576853     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576854     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576855     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576856     5  0.0000      0.913 0.000 0.00 0.000  0 1.000 0.00
#&gt; SRR1576857     3  0.0000      0.988 0.000 0.00 1.000  0 0.000 0.00
#&gt; SRR1576858     3  0.0000      0.988 0.000 0.00 1.000  0 0.000 0.00
#&gt; SRR1576859     3  0.0000      0.988 0.000 0.00 1.000  0 0.000 0.00
#&gt; SRR1576860     3  0.0000      0.988 0.000 0.00 1.000  0 0.000 0.00
#&gt; SRR1576861     3  0.0000      0.988 0.000 0.00 1.000  0 0.000 0.00
#&gt; SRR1576862     3  0.0000      0.988 0.000 0.00 1.000  0 0.000 0.00
#&gt; SRR1576863     5  0.3198      0.815 0.000 0.00 0.000  0 0.740 0.26
#&gt; SRR1576864     5  0.3198      0.815 0.000 0.00 0.000  0 0.740 0.26
#&gt; SRR1576865     5  0.3198      0.815 0.000 0.00 0.000  0 0.740 0.26
#&gt; SRR1576866     5  0.3198      0.815 0.000 0.00 0.000  0 0.740 0.26
#&gt; SRR1576867     1  0.0000      0.962 1.000 0.00 0.000  0 0.000 0.00
#&gt; SRR1576868     1  0.0000      0.962 1.000 0.00 0.000  0 0.000 0.00
#&gt; SRR1576869     1  0.0000      0.962 1.000 0.00 0.000  0 0.000 0.00
#&gt; SRR1576870     1  0.0000      0.962 1.000 0.00 0.000  0 0.000 0.00
#&gt; SRR1576871     1  0.0000      0.962 1.000 0.00 0.000  0 0.000 0.00
#&gt; SRR1576872     1  0.0000      0.962 1.000 0.00 0.000  0 0.000 0.00
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.00 0.000  0 0.000 0.00
#&gt; SRR1576877     1  0.1444      0.929 0.928 0.00 0.000  0 0.072 0.00
#&gt; SRR1576878     1  0.2048      0.887 0.880 0.00 0.000  0 0.120 0.00
#&gt; SRR1576879     1  0.1610      0.921 0.916 0.00 0.000  0 0.084 0.00
#&gt; SRR1576880     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576881     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576882     4  0.0000      1.000 0.000 0.00 0.000  1 0.000 0.00
#&gt; SRR1576883     6  0.3198      1.000 0.000 0.26 0.000  0 0.000 0.74
#&gt; SRR1576884     6  0.3198      1.000 0.000 0.26 0.000  0 0.000 0.74
#&gt; SRR1576885     6  0.3198      1.000 0.000 0.26 0.000  0 0.000 0.74
#&gt; SRR1576886     6  0.3198      1.000 0.000 0.26 0.000  0 0.000 0.74
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.432           0.920       0.913         0.3758 0.648   0.648
#> 3 3 0.929           0.896       0.926         0.7511 0.629   0.449
#> 4 4 0.814           0.890       0.922         0.1225 0.799   0.484
#> 5 5 0.918           0.959       0.966         0.0804 0.950   0.800
#> 6 6 0.914           0.945       0.943         0.0333 0.978   0.889
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 3 5
```

There is also optional best $k$ = 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.506      1.000 0.112 0.888
#&gt; SRR1576834     2   0.506      1.000 0.112 0.888
#&gt; SRR1576835     2   0.506      1.000 0.112 0.888
#&gt; SRR1576836     2   0.506      1.000 0.112 0.888
#&gt; SRR1576837     1   0.343      0.904 0.936 0.064
#&gt; SRR1576838     1   0.343      0.904 0.936 0.064
#&gt; SRR1576839     1   0.343      0.904 0.936 0.064
#&gt; SRR1576840     1   0.343      0.904 0.936 0.064
#&gt; SRR1576841     1   0.615      0.880 0.848 0.152
#&gt; SRR1576842     1   0.615      0.880 0.848 0.152
#&gt; SRR1576843     1   0.615      0.880 0.848 0.152
#&gt; SRR1576844     1   0.000      0.916 1.000 0.000
#&gt; SRR1576845     1   0.000      0.916 1.000 0.000
#&gt; SRR1576846     1   0.000      0.916 1.000 0.000
#&gt; SRR1576847     1   0.000      0.916 1.000 0.000
#&gt; SRR1576848     1   0.000      0.916 1.000 0.000
#&gt; SRR1576849     1   0.000      0.916 1.000 0.000
#&gt; SRR1576850     1   0.000      0.916 1.000 0.000
#&gt; SRR1576851     1   0.000      0.916 1.000 0.000
#&gt; SRR1576852     1   0.000      0.916 1.000 0.000
#&gt; SRR1576853     1   0.343      0.904 0.936 0.064
#&gt; SRR1576854     1   0.343      0.904 0.936 0.064
#&gt; SRR1576855     1   0.343      0.904 0.936 0.064
#&gt; SRR1576856     1   0.343      0.904 0.936 0.064
#&gt; SRR1576857     1   0.689      0.860 0.816 0.184
#&gt; SRR1576858     1   0.689      0.860 0.816 0.184
#&gt; SRR1576859     1   0.689      0.860 0.816 0.184
#&gt; SRR1576860     1   0.689      0.860 0.816 0.184
#&gt; SRR1576861     1   0.689      0.860 0.816 0.184
#&gt; SRR1576862     1   0.689      0.860 0.816 0.184
#&gt; SRR1576863     1   0.563      0.877 0.868 0.132
#&gt; SRR1576864     1   0.563      0.877 0.868 0.132
#&gt; SRR1576865     1   0.563      0.877 0.868 0.132
#&gt; SRR1576866     1   0.563      0.877 0.868 0.132
#&gt; SRR1576867     1   0.584      0.906 0.860 0.140
#&gt; SRR1576868     1   0.584      0.906 0.860 0.140
#&gt; SRR1576869     1   0.584      0.906 0.860 0.140
#&gt; SRR1576870     1   0.584      0.906 0.860 0.140
#&gt; SRR1576871     1   0.584      0.906 0.860 0.140
#&gt; SRR1576872     1   0.584      0.906 0.860 0.140
#&gt; SRR1576873     2   0.506      1.000 0.112 0.888
#&gt; SRR1576874     2   0.506      1.000 0.112 0.888
#&gt; SRR1576875     2   0.506      1.000 0.112 0.888
#&gt; SRR1576876     2   0.506      1.000 0.112 0.888
#&gt; SRR1576877     1   0.563      0.907 0.868 0.132
#&gt; SRR1576878     1   0.563      0.907 0.868 0.132
#&gt; SRR1576879     1   0.563      0.907 0.868 0.132
#&gt; SRR1576880     1   0.000      0.916 1.000 0.000
#&gt; SRR1576881     1   0.000      0.916 1.000 0.000
#&gt; SRR1576882     1   0.000      0.916 1.000 0.000
#&gt; SRR1576883     2   0.506      1.000 0.112 0.888
#&gt; SRR1576884     2   0.506      1.000 0.112 0.888
#&gt; SRR1576885     2   0.506      1.000 0.112 0.888
#&gt; SRR1576886     2   0.506      1.000 0.112 0.888
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576834     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576835     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576836     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576837     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576838     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576839     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576840     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576841     3   0.116      0.933 0.000 0.028 0.972
#&gt; SRR1576842     3   0.103      0.935 0.000 0.024 0.976
#&gt; SRR1576843     3   0.103      0.935 0.000 0.024 0.976
#&gt; SRR1576844     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576845     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576846     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576847     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576848     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576849     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576850     3   0.537      0.686 0.004 0.252 0.744
#&gt; SRR1576851     3   0.537      0.686 0.004 0.252 0.744
#&gt; SRR1576852     3   0.537      0.686 0.004 0.252 0.744
#&gt; SRR1576853     1   0.250      0.964 0.928 0.004 0.068
#&gt; SRR1576854     1   0.250      0.964 0.928 0.004 0.068
#&gt; SRR1576855     1   0.250      0.964 0.928 0.004 0.068
#&gt; SRR1576856     1   0.250      0.964 0.928 0.004 0.068
#&gt; SRR1576857     3   0.176      0.935 0.004 0.040 0.956
#&gt; SRR1576858     3   0.176      0.935 0.004 0.040 0.956
#&gt; SRR1576859     3   0.176      0.935 0.004 0.040 0.956
#&gt; SRR1576860     3   0.176      0.935 0.004 0.040 0.956
#&gt; SRR1576861     3   0.176      0.935 0.004 0.040 0.956
#&gt; SRR1576862     3   0.176      0.935 0.004 0.040 0.956
#&gt; SRR1576863     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576864     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576865     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576866     1   0.268      0.965 0.924 0.008 0.068
#&gt; SRR1576867     1   0.000      0.932 1.000 0.000 0.000
#&gt; SRR1576868     1   0.000      0.932 1.000 0.000 0.000
#&gt; SRR1576869     1   0.000      0.932 1.000 0.000 0.000
#&gt; SRR1576870     1   0.000      0.932 1.000 0.000 0.000
#&gt; SRR1576871     1   0.000      0.932 1.000 0.000 0.000
#&gt; SRR1576872     1   0.000      0.932 1.000 0.000 0.000
#&gt; SRR1576873     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576874     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576875     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576876     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576877     2   0.624      0.448 0.440 0.560 0.000
#&gt; SRR1576878     2   0.624      0.448 0.440 0.560 0.000
#&gt; SRR1576879     2   0.624      0.448 0.440 0.560 0.000
#&gt; SRR1576880     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576881     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576882     3   0.000      0.944 0.000 0.000 1.000
#&gt; SRR1576883     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576884     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576885     2   0.153      0.911 0.040 0.960 0.000
#&gt; SRR1576886     2   0.153      0.911 0.040 0.960 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576837     3  0.4595      0.742 0.176 0.044 0.780 0.000
#&gt; SRR1576838     3  0.4595      0.742 0.176 0.044 0.780 0.000
#&gt; SRR1576839     3  0.4595      0.742 0.176 0.044 0.780 0.000
#&gt; SRR1576840     3  0.4595      0.742 0.176 0.044 0.780 0.000
#&gt; SRR1576841     4  0.0336      0.991 0.000 0.000 0.008 0.992
#&gt; SRR1576842     4  0.0336      0.991 0.000 0.000 0.008 0.992
#&gt; SRR1576843     4  0.0336      0.991 0.000 0.000 0.008 0.992
#&gt; SRR1576844     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576845     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576846     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576847     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576848     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576849     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576850     4  0.0804      0.982 0.008 0.000 0.012 0.980
#&gt; SRR1576851     4  0.0804      0.982 0.008 0.000 0.012 0.980
#&gt; SRR1576852     4  0.0804      0.982 0.008 0.000 0.012 0.980
#&gt; SRR1576853     3  0.3850      0.764 0.116 0.044 0.840 0.000
#&gt; SRR1576854     3  0.3850      0.764 0.116 0.044 0.840 0.000
#&gt; SRR1576855     3  0.3850      0.764 0.116 0.044 0.840 0.000
#&gt; SRR1576856     3  0.3850      0.764 0.116 0.044 0.840 0.000
#&gt; SRR1576857     3  0.4804      0.529 0.000 0.000 0.616 0.384
#&gt; SRR1576858     3  0.4804      0.529 0.000 0.000 0.616 0.384
#&gt; SRR1576859     3  0.4804      0.529 0.000 0.000 0.616 0.384
#&gt; SRR1576860     3  0.4804      0.529 0.000 0.000 0.616 0.384
#&gt; SRR1576861     3  0.4804      0.529 0.000 0.000 0.616 0.384
#&gt; SRR1576862     3  0.4804      0.529 0.000 0.000 0.616 0.384
#&gt; SRR1576863     3  0.1767      0.757 0.012 0.044 0.944 0.000
#&gt; SRR1576864     3  0.1767      0.757 0.012 0.044 0.944 0.000
#&gt; SRR1576865     3  0.1767      0.757 0.012 0.044 0.944 0.000
#&gt; SRR1576866     3  0.1767      0.757 0.012 0.044 0.944 0.000
#&gt; SRR1576867     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576877     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576881     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576882     4  0.0000      0.994 0.000 0.000 0.000 1.000
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.2891      0.901 0.176 0.000 0.000 0.000 0.824
#&gt; SRR1576838     5  0.2891      0.901 0.176 0.000 0.000 0.000 0.824
#&gt; SRR1576839     5  0.2891      0.901 0.176 0.000 0.000 0.000 0.824
#&gt; SRR1576840     5  0.2891      0.901 0.176 0.000 0.000 0.000 0.824
#&gt; SRR1576841     4  0.2690      0.844 0.000 0.000 0.156 0.844 0.000
#&gt; SRR1576842     4  0.2690      0.844 0.000 0.000 0.156 0.844 0.000
#&gt; SRR1576843     4  0.2690      0.844 0.000 0.000 0.156 0.844 0.000
#&gt; SRR1576844     4  0.0000      0.953 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576845     4  0.0000      0.953 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576846     4  0.0000      0.953 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576847     4  0.0609      0.952 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1576848     4  0.0609      0.952 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1576849     4  0.0609      0.952 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1576850     4  0.1377      0.946 0.000 0.004 0.020 0.956 0.020
#&gt; SRR1576851     4  0.1377      0.946 0.000 0.004 0.020 0.956 0.020
#&gt; SRR1576852     4  0.1377      0.946 0.000 0.004 0.020 0.956 0.020
#&gt; SRR1576853     5  0.2179      0.926 0.112 0.000 0.000 0.000 0.888
#&gt; SRR1576854     5  0.2179      0.926 0.112 0.000 0.000 0.000 0.888
#&gt; SRR1576855     5  0.2179      0.926 0.112 0.000 0.000 0.000 0.888
#&gt; SRR1576856     5  0.2179      0.926 0.112 0.000 0.000 0.000 0.888
#&gt; SRR1576857     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576863     5  0.0000      0.886 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576864     5  0.0000      0.886 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576865     5  0.0000      0.886 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576866     5  0.0000      0.886 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576867     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0000      0.953 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576881     4  0.0000      0.953 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576882     4  0.0000      0.953 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     5   0.127      0.944 0.060 0.000 0.000 0.000 0.940 0.000
#&gt; SRR1576838     5   0.127      0.944 0.060 0.000 0.000 0.000 0.940 0.000
#&gt; SRR1576839     5   0.127      0.944 0.060 0.000 0.000 0.000 0.940 0.000
#&gt; SRR1576840     5   0.127      0.944 0.060 0.000 0.000 0.000 0.940 0.000
#&gt; SRR1576841     4   0.242      0.815 0.000 0.000 0.156 0.844 0.000 0.000
#&gt; SRR1576842     4   0.242      0.815 0.000 0.000 0.156 0.844 0.000 0.000
#&gt; SRR1576843     4   0.242      0.815 0.000 0.000 0.156 0.844 0.000 0.000
#&gt; SRR1576844     4   0.000      0.896 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576845     4   0.000      0.896 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576846     4   0.000      0.896 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576847     4   0.283      0.877 0.000 0.000 0.000 0.836 0.020 0.144
#&gt; SRR1576848     4   0.283      0.877 0.000 0.000 0.000 0.836 0.020 0.144
#&gt; SRR1576849     4   0.283      0.877 0.000 0.000 0.000 0.836 0.020 0.144
#&gt; SRR1576850     4   0.327      0.868 0.000 0.008 0.000 0.808 0.020 0.164
#&gt; SRR1576851     4   0.327      0.868 0.000 0.008 0.000 0.808 0.020 0.164
#&gt; SRR1576852     4   0.327      0.868 0.000 0.008 0.000 0.808 0.020 0.164
#&gt; SRR1576853     5   0.133      0.942 0.020 0.000 0.000 0.000 0.948 0.032
#&gt; SRR1576854     5   0.133      0.942 0.020 0.000 0.000 0.000 0.948 0.032
#&gt; SRR1576855     5   0.133      0.942 0.020 0.000 0.000 0.000 0.948 0.032
#&gt; SRR1576856     5   0.133      0.942 0.020 0.000 0.000 0.000 0.948 0.032
#&gt; SRR1576857     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     6   0.230      1.000 0.000 0.000 0.000 0.000 0.144 0.856
#&gt; SRR1576864     6   0.230      1.000 0.000 0.000 0.000 0.000 0.144 0.856
#&gt; SRR1576865     6   0.230      1.000 0.000 0.000 0.000 0.000 0.144 0.856
#&gt; SRR1576866     6   0.230      1.000 0.000 0.000 0.000 0.000 0.144 0.856
#&gt; SRR1576867     1   0.000      0.952 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1   0.000      0.952 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1   0.000      0.952 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1   0.000      0.952 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1   0.000      0.952 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1   0.000      0.952 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576877     1   0.200      0.901 0.884 0.000 0.000 0.000 0.116 0.000
#&gt; SRR1576878     1   0.200      0.901 0.884 0.000 0.000 0.000 0.116 0.000
#&gt; SRR1576879     1   0.200      0.901 0.884 0.000 0.000 0.000 0.116 0.000
#&gt; SRR1576880     4   0.000      0.896 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576881     4   0.000      0.896 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576882     4   0.000      0.896 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576883     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576884     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576885     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576886     2   0.000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.939           0.932       0.963         0.4931 0.497   0.497
#> 3 3 0.690           0.772       0.890         0.2734 0.820   0.657
#> 4 4 0.856           0.916       0.954         0.1979 0.807   0.524
#> 5 5 0.891           0.913       0.951         0.0650 0.857   0.519
#> 6 6 0.968           0.880       0.948         0.0446 0.923   0.652
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576834     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576835     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576836     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576837     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576838     1  0.2043      0.935 0.968 0.032
#&gt; SRR1576839     1  0.5629      0.863 0.868 0.132
#&gt; SRR1576840     1  0.6973      0.800 0.812 0.188
#&gt; SRR1576841     1  0.2778      0.921 0.952 0.048
#&gt; SRR1576842     1  0.2778      0.921 0.952 0.048
#&gt; SRR1576843     1  0.3274      0.914 0.940 0.060
#&gt; SRR1576844     2  0.0672      0.983 0.008 0.992
#&gt; SRR1576845     2  0.0672      0.983 0.008 0.992
#&gt; SRR1576846     2  0.0672      0.983 0.008 0.992
#&gt; SRR1576847     2  0.1414      0.974 0.020 0.980
#&gt; SRR1576848     2  0.1414      0.974 0.020 0.980
#&gt; SRR1576849     2  0.1414      0.974 0.020 0.980
#&gt; SRR1576850     2  0.0376      0.984 0.004 0.996
#&gt; SRR1576851     2  0.0376      0.984 0.004 0.996
#&gt; SRR1576852     2  0.0376      0.984 0.004 0.996
#&gt; SRR1576853     1  0.9732      0.409 0.596 0.404
#&gt; SRR1576854     1  0.9881      0.322 0.564 0.436
#&gt; SRR1576855     2  0.2778      0.947 0.048 0.952
#&gt; SRR1576856     2  0.4298      0.902 0.088 0.912
#&gt; SRR1576857     1  0.1633      0.931 0.976 0.024
#&gt; SRR1576858     1  0.1633      0.931 0.976 0.024
#&gt; SRR1576859     1  0.1184      0.932 0.984 0.016
#&gt; SRR1576860     1  0.0000      0.931 1.000 0.000
#&gt; SRR1576861     1  0.0000      0.931 1.000 0.000
#&gt; SRR1576862     1  0.0000      0.931 1.000 0.000
#&gt; SRR1576863     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576864     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576865     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576866     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576867     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576868     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576869     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576870     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576871     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576872     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576873     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576874     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576875     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576876     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576877     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576878     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576879     1  0.1414      0.938 0.980 0.020
#&gt; SRR1576880     2  0.3114      0.943 0.056 0.944
#&gt; SRR1576881     2  0.4022      0.916 0.080 0.920
#&gt; SRR1576882     2  0.2043      0.965 0.032 0.968
#&gt; SRR1576883     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576884     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576885     2  0.0000      0.985 0.000 1.000
#&gt; SRR1576886     2  0.0000      0.985 0.000 1.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576837     1  0.0424      0.813 0.992 0.000 0.008
#&gt; SRR1576838     1  0.0848      0.812 0.984 0.008 0.008
#&gt; SRR1576839     1  0.6282      0.529 0.664 0.324 0.012
#&gt; SRR1576840     1  0.6688      0.316 0.580 0.408 0.012
#&gt; SRR1576841     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576842     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576843     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576844     2  0.5591      0.629 0.000 0.696 0.304
#&gt; SRR1576845     2  0.5560      0.635 0.000 0.700 0.300
#&gt; SRR1576846     2  0.5465      0.652 0.000 0.712 0.288
#&gt; SRR1576847     3  0.6309     -0.269 0.000 0.500 0.500
#&gt; SRR1576848     2  0.6235      0.358 0.000 0.564 0.436
#&gt; SRR1576849     2  0.6302      0.227 0.000 0.520 0.480
#&gt; SRR1576850     2  0.1337      0.861 0.016 0.972 0.012
#&gt; SRR1576851     2  0.1015      0.863 0.008 0.980 0.012
#&gt; SRR1576852     2  0.1905      0.857 0.016 0.956 0.028
#&gt; SRR1576853     1  0.4235      0.740 0.824 0.176 0.000
#&gt; SRR1576854     1  0.4605      0.719 0.796 0.204 0.000
#&gt; SRR1576855     1  0.4887      0.690 0.772 0.228 0.000
#&gt; SRR1576856     1  0.4291      0.740 0.820 0.180 0.000
#&gt; SRR1576857     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576858     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576859     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576860     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576861     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576862     3  0.0000      0.925 0.000 0.000 1.000
#&gt; SRR1576863     2  0.2796      0.818 0.092 0.908 0.000
#&gt; SRR1576864     2  0.2796      0.818 0.092 0.908 0.000
#&gt; SRR1576865     2  0.2796      0.818 0.092 0.908 0.000
#&gt; SRR1576866     2  0.2796      0.818 0.092 0.908 0.000
#&gt; SRR1576867     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576868     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576869     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576870     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576871     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576872     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576873     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576877     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576878     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576879     1  0.2796      0.832 0.908 0.000 0.092
#&gt; SRR1576880     2  0.5431      0.657 0.000 0.716 0.284
#&gt; SRR1576881     2  0.5363      0.666 0.000 0.724 0.276
#&gt; SRR1576882     2  0.5016      0.703 0.000 0.760 0.240
#&gt; SRR1576883     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576884     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576885     2  0.0000      0.867 0.000 1.000 0.000
#&gt; SRR1576886     2  0.0000      0.867 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576834     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576835     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576836     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576837     1  0.0188      0.929 0.996 0.000 0.000 0.004
#&gt; SRR1576838     1  0.0188      0.929 0.996 0.000 0.000 0.004
#&gt; SRR1576839     1  0.4790      0.431 0.620 0.000 0.000 0.380
#&gt; SRR1576840     1  0.4776      0.440 0.624 0.000 0.000 0.376
#&gt; SRR1576841     3  0.0657      0.954 0.000 0.012 0.984 0.004
#&gt; SRR1576842     3  0.0657      0.954 0.000 0.012 0.984 0.004
#&gt; SRR1576843     3  0.0657      0.954 0.000 0.012 0.984 0.004
#&gt; SRR1576844     2  0.3402      0.838 0.000 0.832 0.164 0.004
#&gt; SRR1576845     2  0.3306      0.845 0.000 0.840 0.156 0.004
#&gt; SRR1576846     2  0.2831      0.870 0.000 0.876 0.120 0.004
#&gt; SRR1576847     3  0.2714      0.879 0.000 0.112 0.884 0.004
#&gt; SRR1576848     3  0.3257      0.829 0.000 0.152 0.844 0.004
#&gt; SRR1576849     3  0.2654      0.883 0.000 0.108 0.888 0.004
#&gt; SRR1576850     4  0.0000      0.992 0.000 0.000 0.000 1.000
#&gt; SRR1576851     4  0.0000      0.992 0.000 0.000 0.000 1.000
#&gt; SRR1576852     4  0.0000      0.992 0.000 0.000 0.000 1.000
#&gt; SRR1576853     4  0.0592      0.985 0.016 0.000 0.000 0.984
#&gt; SRR1576854     4  0.0592      0.985 0.016 0.000 0.000 0.984
#&gt; SRR1576855     4  0.0657      0.988 0.012 0.004 0.000 0.984
#&gt; SRR1576856     4  0.0524      0.990 0.008 0.004 0.000 0.988
#&gt; SRR1576857     3  0.0000      0.957 0.000 0.000 1.000 0.000
#&gt; SRR1576858     3  0.0000      0.957 0.000 0.000 1.000 0.000
#&gt; SRR1576859     3  0.0000      0.957 0.000 0.000 1.000 0.000
#&gt; SRR1576860     3  0.0000      0.957 0.000 0.000 1.000 0.000
#&gt; SRR1576861     3  0.0000      0.957 0.000 0.000 1.000 0.000
#&gt; SRR1576862     3  0.0000      0.957 0.000 0.000 1.000 0.000
#&gt; SRR1576863     4  0.0188      0.992 0.000 0.004 0.000 0.996
#&gt; SRR1576864     4  0.0188      0.992 0.000 0.004 0.000 0.996
#&gt; SRR1576865     4  0.0188      0.992 0.000 0.004 0.000 0.996
#&gt; SRR1576866     4  0.0188      0.992 0.000 0.004 0.000 0.996
#&gt; SRR1576867     1  0.0000      0.930 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.930 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.930 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.930 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.930 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.930 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576874     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576875     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576876     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576877     1  0.0469      0.925 0.988 0.012 0.000 0.000
#&gt; SRR1576878     1  0.0469      0.925 0.988 0.012 0.000 0.000
#&gt; SRR1576879     1  0.0469      0.925 0.988 0.012 0.000 0.000
#&gt; SRR1576880     2  0.3494      0.829 0.000 0.824 0.172 0.004
#&gt; SRR1576881     2  0.3448      0.834 0.000 0.828 0.168 0.004
#&gt; SRR1576882     2  0.3208      0.852 0.000 0.848 0.148 0.004
#&gt; SRR1576883     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576884     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576885     2  0.0469      0.936 0.000 0.988 0.000 0.012
#&gt; SRR1576886     2  0.0469      0.936 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576838     1  0.0162      0.880 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576839     1  0.6269      0.504 0.616 0.132 0.220 0.000 0.032
#&gt; SRR1576840     1  0.6780      0.401 0.544 0.212 0.216 0.000 0.028
#&gt; SRR1576841     4  0.1410      0.891 0.000 0.000 0.060 0.940 0.000
#&gt; SRR1576842     4  0.1341      0.893 0.000 0.000 0.056 0.944 0.000
#&gt; SRR1576843     4  0.1908      0.871 0.000 0.000 0.092 0.908 0.000
#&gt; SRR1576844     4  0.0000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576845     4  0.0000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576846     4  0.0000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576847     4  0.2890      0.836 0.000 0.000 0.160 0.836 0.004
#&gt; SRR1576848     4  0.2890      0.836 0.000 0.000 0.160 0.836 0.004
#&gt; SRR1576849     4  0.2890      0.836 0.000 0.000 0.160 0.836 0.004
#&gt; SRR1576850     4  0.3039      0.808 0.000 0.000 0.000 0.808 0.192
#&gt; SRR1576851     4  0.2929      0.817 0.000 0.000 0.000 0.820 0.180
#&gt; SRR1576852     4  0.3039      0.808 0.000 0.000 0.000 0.808 0.192
#&gt; SRR1576853     5  0.0162      0.995 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1576854     5  0.0162      0.995 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1576855     5  0.0162      0.998 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1576856     5  0.0162      0.998 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1576857     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576863     5  0.0162      0.998 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1576864     5  0.0162      0.998 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1576865     5  0.0162      0.998 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1576866     5  0.0162      0.998 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1576867     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.882 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.2813      0.790 0.832 0.000 0.000 0.168 0.000
#&gt; SRR1576878     1  0.2773      0.792 0.836 0.000 0.000 0.164 0.000
#&gt; SRR1576879     1  0.2929      0.780 0.820 0.000 0.000 0.180 0.000
#&gt; SRR1576880     4  0.0000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576881     4  0.0000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576882     4  0.0000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.1010      0.884 0.036 0.000 0.000 0.000 0.960 0.004
#&gt; SRR1576838     5  0.1010      0.884 0.036 0.000 0.000 0.000 0.960 0.004
#&gt; SRR1576839     5  0.5039      0.639 0.036 0.240 0.000 0.000 0.664 0.060
#&gt; SRR1576840     5  0.5129      0.432 0.036 0.364 0.000 0.000 0.568 0.032
#&gt; SRR1576841     1  0.1151      0.876 0.956 0.000 0.012 0.032 0.000 0.000
#&gt; SRR1576842     1  0.1049      0.876 0.960 0.000 0.008 0.032 0.000 0.000
#&gt; SRR1576843     1  0.1789      0.861 0.924 0.000 0.044 0.032 0.000 0.000
#&gt; SRR1576844     1  0.3864      0.104 0.520 0.000 0.000 0.480 0.000 0.000
#&gt; SRR1576845     1  0.3747      0.359 0.604 0.000 0.000 0.396 0.000 0.000
#&gt; SRR1576846     4  0.3868     -0.224 0.496 0.000 0.000 0.504 0.000 0.000
#&gt; SRR1576847     4  0.0146      0.897 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; SRR1576848     4  0.0146      0.897 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; SRR1576849     4  0.0146      0.897 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; SRR1576850     4  0.0000      0.897 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576851     4  0.0000      0.897 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576852     4  0.0000      0.897 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576853     6  0.0865      0.979 0.036 0.000 0.000 0.000 0.000 0.964
#&gt; SRR1576854     6  0.0865      0.979 0.036 0.000 0.000 0.000 0.000 0.964
#&gt; SRR1576855     6  0.0865      0.979 0.036 0.000 0.000 0.000 0.000 0.964
#&gt; SRR1576856     6  0.0865      0.979 0.036 0.000 0.000 0.000 0.000 0.964
#&gt; SRR1576857     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     6  0.0146      0.979 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1576864     6  0.0146      0.979 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1576865     6  0.0146      0.979 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1576866     6  0.0146      0.979 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1576867     5  0.0000      0.900 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576868     5  0.0000      0.900 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576869     5  0.0000      0.900 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576870     5  0.0000      0.900 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576871     5  0.0000      0.900 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576872     5  0.0000      0.900 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0937      0.864 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1576878     1  0.0937      0.864 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1576879     1  0.0937      0.864 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1576880     1  0.0865      0.876 0.964 0.000 0.000 0.036 0.000 0.000
#&gt; SRR1576881     1  0.0937      0.876 0.960 0.000 0.000 0.040 0.000 0.000
#&gt; SRR1576882     1  0.1327      0.864 0.936 0.000 0.000 0.064 0.000 0.000
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.516           0.739       0.849         0.4723 0.516   0.516
#> 3 3 0.723           0.882       0.913         0.2416 0.849   0.707
#> 4 4 0.774           0.923       0.956         0.1992 0.937   0.828
#> 5 5 1.000           0.951       0.979         0.1430 0.899   0.667
#> 6 6 1.000           0.951       0.979         0.0278 0.978   0.889
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576834     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576835     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576836     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576837     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576838     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576839     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576840     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576841     2  0.0672      0.689 0.008 0.992
#&gt; SRR1576842     2  0.0672      0.689 0.008 0.992
#&gt; SRR1576843     2  0.0672      0.689 0.008 0.992
#&gt; SRR1576844     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576845     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576846     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576847     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576848     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576849     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576850     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576851     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576852     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576853     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576854     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576855     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576856     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576857     2  0.0000      0.686 0.000 1.000
#&gt; SRR1576858     2  0.0000      0.686 0.000 1.000
#&gt; SRR1576859     2  0.0000      0.686 0.000 1.000
#&gt; SRR1576860     2  0.0000      0.686 0.000 1.000
#&gt; SRR1576861     2  0.0000      0.686 0.000 1.000
#&gt; SRR1576862     2  0.0000      0.686 0.000 1.000
#&gt; SRR1576863     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576864     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576865     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576866     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576867     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576868     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576869     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576870     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576871     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576872     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576873     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576874     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576875     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576876     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576877     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576878     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576879     1  0.9635      0.541 0.612 0.388
#&gt; SRR1576880     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576881     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576882     2  0.9635      0.722 0.388 0.612
#&gt; SRR1576883     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576884     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576885     1  0.0000      0.842 1.000 0.000
#&gt; SRR1576886     1  0.0000      0.842 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1576833     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576834     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576835     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576836     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576837     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576838     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576839     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576840     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576841     3  0.0424      0.674  0 0.008 0.992
#&gt; SRR1576842     3  0.0424      0.674  0 0.008 0.992
#&gt; SRR1576843     3  0.0424      0.674  0 0.008 0.992
#&gt; SRR1576844     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576845     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576846     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576847     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576848     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576849     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576850     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576851     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576852     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576853     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576854     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576855     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576856     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576857     3  0.0000      0.670  0 0.000 1.000
#&gt; SRR1576858     3  0.0000      0.670  0 0.000 1.000
#&gt; SRR1576859     3  0.0000      0.670  0 0.000 1.000
#&gt; SRR1576860     3  0.0000      0.670  0 0.000 1.000
#&gt; SRR1576861     3  0.0000      0.670  0 0.000 1.000
#&gt; SRR1576862     3  0.0000      0.670  0 0.000 1.000
#&gt; SRR1576863     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576864     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576865     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576866     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576867     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576873     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576874     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576875     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576876     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576877     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1576880     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576881     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576882     3  0.6079      0.717  0 0.388 0.612
#&gt; SRR1576883     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576884     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576885     2  0.0000      1.000  0 1.000 0.000
#&gt; SRR1576886     2  0.0000      1.000  0 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2   p3    p4
#&gt; SRR1576833     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576834     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576835     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576836     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576837     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576838     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576839     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576840     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576841     4   0.479      0.464  0 0.000 0.38 0.620
#&gt; SRR1576842     4   0.479      0.464  0 0.000 0.38 0.620
#&gt; SRR1576843     4   0.479      0.464  0 0.000 0.38 0.620
#&gt; SRR1576844     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576845     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576846     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576847     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576848     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576849     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576850     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576851     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576852     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576853     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576854     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576855     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576856     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576857     3   0.000      1.000  0 0.000 1.00 0.000
#&gt; SRR1576858     3   0.000      1.000  0 0.000 1.00 0.000
#&gt; SRR1576859     3   0.000      1.000  0 0.000 1.00 0.000
#&gt; SRR1576860     3   0.000      1.000  0 0.000 1.00 0.000
#&gt; SRR1576861     3   0.000      1.000  0 0.000 1.00 0.000
#&gt; SRR1576862     3   0.000      1.000  0 0.000 1.00 0.000
#&gt; SRR1576863     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576864     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576865     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576866     2   0.000      0.939  0 1.000 0.00 0.000
#&gt; SRR1576867     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576868     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576869     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576870     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576871     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576872     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576873     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576874     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576875     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576876     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576877     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576878     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576879     1   0.000      1.000  1 0.000 0.00 0.000
#&gt; SRR1576880     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576881     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576882     4   0.000      0.911  0 0.000 0.00 1.000
#&gt; SRR1576883     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576884     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576885     2   0.241      0.939  0 0.896 0.00 0.104
#&gt; SRR1576886     2   0.241      0.939  0 0.896 0.00 0.104
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2   p3   p4 p5
#&gt; SRR1576833     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576834     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576835     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576836     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576837     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576838     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576839     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576840     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576841     4   0.413      0.475  0  0 0.38 0.62  0
#&gt; SRR1576842     4   0.413      0.475  0  0 0.38 0.62  0
#&gt; SRR1576843     4   0.413      0.475  0  0 0.38 0.62  0
#&gt; SRR1576844     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576845     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576846     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576847     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576848     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576849     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576850     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576851     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576852     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576853     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576854     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576855     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576856     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576857     3   0.000      1.000  0  0 1.00 0.00  0
#&gt; SRR1576858     3   0.000      1.000  0  0 1.00 0.00  0
#&gt; SRR1576859     3   0.000      1.000  0  0 1.00 0.00  0
#&gt; SRR1576860     3   0.000      1.000  0  0 1.00 0.00  0
#&gt; SRR1576861     3   0.000      1.000  0  0 1.00 0.00  0
#&gt; SRR1576862     3   0.000      1.000  0  0 1.00 0.00  0
#&gt; SRR1576863     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576864     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576865     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576866     5   0.000      1.000  0  0 0.00 0.00  1
#&gt; SRR1576867     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576868     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576869     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576870     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576871     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576872     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576873     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576874     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576875     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576876     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576877     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576878     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576879     1   0.000      1.000  1  0 0.00 0.00  0
#&gt; SRR1576880     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576881     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576882     4   0.000      0.912  0  0 0.00 1.00  0
#&gt; SRR1576883     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576884     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576885     2   0.000      1.000  0  1 0.00 0.00  0
#&gt; SRR1576886     2   0.000      1.000  0  1 0.00 0.00  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2   p3   p4 p5 p6
#&gt; SRR1576833     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576834     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576835     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576836     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576837     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576838     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576839     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576840     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576841     4   0.371      0.475  0  0 0.38 0.62  0  0
#&gt; SRR1576842     4   0.371      0.475  0  0 0.38 0.62  0  0
#&gt; SRR1576843     4   0.371      0.475  0  0 0.38 0.62  0  0
#&gt; SRR1576844     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576845     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576846     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576847     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576848     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576849     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576850     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576851     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576852     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576853     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576854     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576855     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576856     5   0.000      1.000  0  0 0.00 0.00  1  0
#&gt; SRR1576857     3   0.000      1.000  0  0 1.00 0.00  0  0
#&gt; SRR1576858     3   0.000      1.000  0  0 1.00 0.00  0  0
#&gt; SRR1576859     3   0.000      1.000  0  0 1.00 0.00  0  0
#&gt; SRR1576860     3   0.000      1.000  0  0 1.00 0.00  0  0
#&gt; SRR1576861     3   0.000      1.000  0  0 1.00 0.00  0  0
#&gt; SRR1576862     3   0.000      1.000  0  0 1.00 0.00  0  0
#&gt; SRR1576863     6   0.000      1.000  0  0 0.00 0.00  0  1
#&gt; SRR1576864     6   0.000      1.000  0  0 0.00 0.00  0  1
#&gt; SRR1576865     6   0.000      1.000  0  0 0.00 0.00  0  1
#&gt; SRR1576866     6   0.000      1.000  0  0 0.00 0.00  0  1
#&gt; SRR1576867     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576868     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576869     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576870     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576871     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576872     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576873     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576874     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576875     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576876     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576877     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576878     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576879     1   0.000      1.000  1  0 0.00 0.00  0  0
#&gt; SRR1576880     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576881     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576882     4   0.000      0.910  0  0 0.00 1.00  0  0
#&gt; SRR1576883     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576884     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576885     2   0.000      1.000  0  1 0.00 0.00  0  0
#&gt; SRR1576886     2   0.000      1.000  0  1 0.00 0.00  0  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.184           0.568       0.723         0.4556 0.560   0.560
#> 3 3 0.308           0.643       0.668         0.3520 0.765   0.581
#> 4 4 0.457           0.669       0.714         0.1504 0.937   0.807
#> 5 5 0.622           0.770       0.778         0.0887 0.894   0.627
#> 6 6 0.757           0.805       0.762         0.0522 0.978   0.889
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576834     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576835     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576836     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576837     1  0.7056    0.82064 0.808 0.192
#&gt; SRR1576838     1  0.7056    0.82064 0.808 0.192
#&gt; SRR1576839     1  0.7815    0.75741 0.768 0.232
#&gt; SRR1576840     1  0.7815    0.75741 0.768 0.232
#&gt; SRR1576841     2  0.9358    0.25995 0.352 0.648
#&gt; SRR1576842     2  0.9358    0.25995 0.352 0.648
#&gt; SRR1576843     2  0.9358    0.25995 0.352 0.648
#&gt; SRR1576844     2  0.0938    0.61930 0.012 0.988
#&gt; SRR1576845     2  0.0938    0.61930 0.012 0.988
#&gt; SRR1576846     2  0.0938    0.61930 0.012 0.988
#&gt; SRR1576847     2  0.1414    0.61983 0.020 0.980
#&gt; SRR1576848     2  0.1414    0.61983 0.020 0.980
#&gt; SRR1576849     2  0.1414    0.61983 0.020 0.980
#&gt; SRR1576850     2  0.2778    0.61140 0.048 0.952
#&gt; SRR1576851     2  0.2778    0.61140 0.048 0.952
#&gt; SRR1576852     2  0.2778    0.61140 0.048 0.952
#&gt; SRR1576853     1  0.6531    0.82116 0.832 0.168
#&gt; SRR1576854     1  0.6531    0.82116 0.832 0.168
#&gt; SRR1576855     1  0.6531    0.82116 0.832 0.168
#&gt; SRR1576856     1  0.6531    0.82116 0.832 0.168
#&gt; SRR1576857     2  1.0000   -0.00435 0.496 0.504
#&gt; SRR1576858     2  1.0000   -0.00435 0.496 0.504
#&gt; SRR1576859     2  1.0000   -0.00435 0.496 0.504
#&gt; SRR1576860     2  1.0000   -0.00435 0.496 0.504
#&gt; SRR1576861     2  1.0000   -0.00435 0.496 0.504
#&gt; SRR1576862     2  1.0000   -0.00435 0.496 0.504
#&gt; SRR1576863     2  0.9686    0.43795 0.396 0.604
#&gt; SRR1576864     2  0.9686    0.43795 0.396 0.604
#&gt; SRR1576865     2  0.9686    0.43795 0.396 0.604
#&gt; SRR1576866     2  0.9686    0.43795 0.396 0.604
#&gt; SRR1576867     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576868     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576869     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576870     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576871     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576872     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576873     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576874     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576875     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576876     2  0.8608    0.55123 0.284 0.716
#&gt; SRR1576877     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576878     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576879     1  0.4298    0.87327 0.912 0.088
#&gt; SRR1576880     2  0.1414    0.61983 0.020 0.980
#&gt; SRR1576881     2  0.1414    0.61983 0.020 0.980
#&gt; SRR1576882     2  0.1414    0.61983 0.020 0.980
#&gt; SRR1576883     2  0.9000    0.51514 0.316 0.684
#&gt; SRR1576884     2  0.9000    0.51514 0.316 0.684
#&gt; SRR1576885     2  0.9000    0.51514 0.316 0.684
#&gt; SRR1576886     2  0.9000    0.51514 0.316 0.684
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576834     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576835     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576836     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576837     1  0.8556      0.704 0.596 0.256 0.148
#&gt; SRR1576838     1  0.8556      0.704 0.596 0.256 0.148
#&gt; SRR1576839     1  0.8722      0.686 0.576 0.272 0.152
#&gt; SRR1576840     1  0.8722      0.686 0.576 0.272 0.152
#&gt; SRR1576841     3  0.7564      0.524 0.156 0.152 0.692
#&gt; SRR1576842     3  0.7564      0.524 0.156 0.152 0.692
#&gt; SRR1576843     3  0.7564      0.524 0.156 0.152 0.692
#&gt; SRR1576844     3  0.6950      0.490 0.016 0.476 0.508
#&gt; SRR1576845     3  0.6950      0.490 0.016 0.476 0.508
#&gt; SRR1576846     3  0.6950      0.490 0.016 0.476 0.508
#&gt; SRR1576847     3  0.6925      0.501 0.016 0.452 0.532
#&gt; SRR1576848     3  0.6925      0.501 0.016 0.452 0.532
#&gt; SRR1576849     3  0.6925      0.501 0.016 0.452 0.532
#&gt; SRR1576850     3  0.6483      0.458 0.008 0.392 0.600
#&gt; SRR1576851     3  0.6483      0.458 0.008 0.392 0.600
#&gt; SRR1576852     3  0.6483      0.458 0.008 0.392 0.600
#&gt; SRR1576853     1  0.9081      0.641 0.552 0.236 0.212
#&gt; SRR1576854     1  0.9081      0.641 0.552 0.236 0.212
#&gt; SRR1576855     1  0.9081      0.641 0.552 0.236 0.212
#&gt; SRR1576856     1  0.9081      0.641 0.552 0.236 0.212
#&gt; SRR1576857     3  0.7924      0.350 0.304 0.084 0.612
#&gt; SRR1576858     3  0.7924      0.350 0.304 0.084 0.612
#&gt; SRR1576859     3  0.7924      0.350 0.304 0.084 0.612
#&gt; SRR1576860     3  0.7924      0.350 0.304 0.084 0.612
#&gt; SRR1576861     3  0.7924      0.350 0.304 0.084 0.612
#&gt; SRR1576862     3  0.7924      0.350 0.304 0.084 0.612
#&gt; SRR1576863     2  0.7782      0.622 0.124 0.668 0.208
#&gt; SRR1576864     2  0.7782      0.622 0.124 0.668 0.208
#&gt; SRR1576865     2  0.7782      0.622 0.124 0.668 0.208
#&gt; SRR1576866     2  0.7782      0.622 0.124 0.668 0.208
#&gt; SRR1576867     1  0.3207      0.787 0.904 0.084 0.012
#&gt; SRR1576868     1  0.3207      0.787 0.904 0.084 0.012
#&gt; SRR1576869     1  0.3207      0.787 0.904 0.084 0.012
#&gt; SRR1576870     1  0.3120      0.785 0.908 0.080 0.012
#&gt; SRR1576871     1  0.3120      0.785 0.908 0.080 0.012
#&gt; SRR1576872     1  0.3120      0.785 0.908 0.080 0.012
#&gt; SRR1576873     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576874     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576875     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576876     2  0.1774      0.859 0.016 0.960 0.024
#&gt; SRR1576877     1  0.3889      0.785 0.884 0.084 0.032
#&gt; SRR1576878     1  0.3889      0.785 0.884 0.084 0.032
#&gt; SRR1576879     1  0.3889      0.785 0.884 0.084 0.032
#&gt; SRR1576880     3  0.6941      0.498 0.016 0.464 0.520
#&gt; SRR1576881     3  0.6941      0.498 0.016 0.464 0.520
#&gt; SRR1576882     3  0.6941      0.498 0.016 0.464 0.520
#&gt; SRR1576883     2  0.0829      0.853 0.012 0.984 0.004
#&gt; SRR1576884     2  0.0829      0.853 0.012 0.984 0.004
#&gt; SRR1576885     2  0.0829      0.853 0.012 0.984 0.004
#&gt; SRR1576886     2  0.0829      0.853 0.012 0.984 0.004
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.5114     0.7750 0.020 0.712 0.008 0.260
#&gt; SRR1576834     2  0.5114     0.7750 0.020 0.712 0.008 0.260
#&gt; SRR1576835     2  0.5114     0.7750 0.020 0.712 0.008 0.260
#&gt; SRR1576836     2  0.5114     0.7750 0.020 0.712 0.008 0.260
#&gt; SRR1576837     1  0.8733     0.5620 0.468 0.244 0.224 0.064
#&gt; SRR1576838     1  0.8733     0.5620 0.468 0.244 0.224 0.064
#&gt; SRR1576839     1  0.8751     0.5598 0.464 0.248 0.224 0.064
#&gt; SRR1576840     1  0.8751     0.5598 0.464 0.248 0.224 0.064
#&gt; SRR1576841     4  0.7625    -0.0586 0.064 0.068 0.328 0.540
#&gt; SRR1576842     4  0.7625    -0.0586 0.064 0.068 0.328 0.540
#&gt; SRR1576843     4  0.7625    -0.0586 0.064 0.068 0.328 0.540
#&gt; SRR1576844     4  0.1211     0.7975 0.000 0.040 0.000 0.960
#&gt; SRR1576845     4  0.1211     0.7975 0.000 0.040 0.000 0.960
#&gt; SRR1576846     4  0.1211     0.7975 0.000 0.040 0.000 0.960
#&gt; SRR1576847     4  0.1798     0.7970 0.000 0.016 0.040 0.944
#&gt; SRR1576848     4  0.1798     0.7970 0.000 0.016 0.040 0.944
#&gt; SRR1576849     4  0.1798     0.7970 0.000 0.016 0.040 0.944
#&gt; SRR1576850     4  0.3547     0.7516 0.000 0.072 0.064 0.864
#&gt; SRR1576851     4  0.3547     0.7516 0.000 0.072 0.064 0.864
#&gt; SRR1576852     4  0.3547     0.7516 0.000 0.072 0.064 0.864
#&gt; SRR1576853     1  0.8599     0.5554 0.420 0.264 0.280 0.036
#&gt; SRR1576854     1  0.8599     0.5554 0.420 0.264 0.280 0.036
#&gt; SRR1576855     1  0.8599     0.5554 0.420 0.264 0.280 0.036
#&gt; SRR1576856     1  0.8599     0.5554 0.420 0.264 0.280 0.036
#&gt; SRR1576857     3  0.6872     1.0000 0.160 0.008 0.624 0.208
#&gt; SRR1576858     3  0.6872     1.0000 0.160 0.008 0.624 0.208
#&gt; SRR1576859     3  0.6872     1.0000 0.160 0.008 0.624 0.208
#&gt; SRR1576860     3  0.6872     1.0000 0.160 0.008 0.624 0.208
#&gt; SRR1576861     3  0.6872     1.0000 0.160 0.008 0.624 0.208
#&gt; SRR1576862     3  0.6872     1.0000 0.160 0.008 0.624 0.208
#&gt; SRR1576863     2  0.9157     0.3491 0.100 0.428 0.272 0.200
#&gt; SRR1576864     2  0.9157     0.3491 0.100 0.428 0.272 0.200
#&gt; SRR1576865     2  0.9157     0.3491 0.100 0.428 0.272 0.200
#&gt; SRR1576866     2  0.9157     0.3491 0.100 0.428 0.272 0.200
#&gt; SRR1576867     1  0.0712     0.6429 0.984 0.004 0.004 0.008
#&gt; SRR1576868     1  0.0712     0.6429 0.984 0.004 0.004 0.008
#&gt; SRR1576869     1  0.0712     0.6429 0.984 0.004 0.004 0.008
#&gt; SRR1576870     1  0.0859     0.6429 0.980 0.004 0.008 0.008
#&gt; SRR1576871     1  0.0859     0.6429 0.980 0.004 0.008 0.008
#&gt; SRR1576872     1  0.0859     0.6429 0.980 0.004 0.008 0.008
#&gt; SRR1576873     2  0.5114     0.7757 0.020 0.712 0.008 0.260
#&gt; SRR1576874     2  0.5114     0.7757 0.020 0.712 0.008 0.260
#&gt; SRR1576875     2  0.5114     0.7757 0.020 0.712 0.008 0.260
#&gt; SRR1576876     2  0.5114     0.7757 0.020 0.712 0.008 0.260
#&gt; SRR1576877     1  0.2742     0.6325 0.912 0.040 0.040 0.008
#&gt; SRR1576878     1  0.2742     0.6325 0.912 0.040 0.040 0.008
#&gt; SRR1576879     1  0.2742     0.6325 0.912 0.040 0.040 0.008
#&gt; SRR1576880     4  0.1151     0.8021 0.000 0.024 0.008 0.968
#&gt; SRR1576881     4  0.1151     0.8021 0.000 0.024 0.008 0.968
#&gt; SRR1576882     4  0.1151     0.8021 0.000 0.024 0.008 0.968
#&gt; SRR1576883     2  0.6243     0.7592 0.020 0.672 0.064 0.244
#&gt; SRR1576884     2  0.6243     0.7592 0.020 0.672 0.064 0.244
#&gt; SRR1576885     2  0.6243     0.7592 0.020 0.672 0.064 0.244
#&gt; SRR1576886     2  0.6243     0.7592 0.020 0.672 0.064 0.244
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0968     0.9208 0.000 0.972 0.012 0.012 0.004
#&gt; SRR1576834     2  0.0968     0.9208 0.000 0.972 0.012 0.012 0.004
#&gt; SRR1576835     2  0.0968     0.9208 0.000 0.972 0.012 0.012 0.004
#&gt; SRR1576836     2  0.0968     0.9208 0.000 0.972 0.012 0.012 0.004
#&gt; SRR1576837     5  0.8621     0.6183 0.256 0.168 0.220 0.008 0.348
#&gt; SRR1576838     5  0.8621     0.6183 0.256 0.168 0.220 0.008 0.348
#&gt; SRR1576839     5  0.8631     0.6195 0.252 0.172 0.220 0.008 0.348
#&gt; SRR1576840     5  0.8631     0.6195 0.252 0.172 0.220 0.008 0.348
#&gt; SRR1576841     4  0.6606     0.0368 0.012 0.028 0.384 0.500 0.076
#&gt; SRR1576842     4  0.6606     0.0368 0.012 0.028 0.384 0.500 0.076
#&gt; SRR1576843     4  0.6606     0.0368 0.012 0.028 0.384 0.500 0.076
#&gt; SRR1576844     4  0.2805     0.7949 0.004 0.124 0.004 0.864 0.004
#&gt; SRR1576845     4  0.2805     0.7949 0.004 0.124 0.004 0.864 0.004
#&gt; SRR1576846     4  0.2805     0.7949 0.004 0.124 0.004 0.864 0.004
#&gt; SRR1576847     4  0.5211     0.7837 0.008 0.124 0.032 0.748 0.088
#&gt; SRR1576848     4  0.5211     0.7837 0.008 0.124 0.032 0.748 0.088
#&gt; SRR1576849     4  0.5211     0.7837 0.008 0.124 0.032 0.748 0.088
#&gt; SRR1576850     4  0.5423     0.7596 0.008 0.088 0.032 0.728 0.144
#&gt; SRR1576851     4  0.5423     0.7596 0.008 0.088 0.032 0.728 0.144
#&gt; SRR1576852     4  0.5423     0.7596 0.008 0.088 0.032 0.728 0.144
#&gt; SRR1576853     5  0.7386     0.6700 0.224 0.088 0.168 0.000 0.520
#&gt; SRR1576854     5  0.7386     0.6700 0.224 0.088 0.168 0.000 0.520
#&gt; SRR1576855     5  0.7386     0.6700 0.224 0.088 0.168 0.000 0.520
#&gt; SRR1576856     5  0.7386     0.6700 0.224 0.088 0.168 0.000 0.520
#&gt; SRR1576857     3  0.3730     0.9927 0.048 0.028 0.840 0.084 0.000
#&gt; SRR1576858     3  0.3730     0.9927 0.048 0.028 0.840 0.084 0.000
#&gt; SRR1576859     3  0.3730     0.9927 0.048 0.028 0.840 0.084 0.000
#&gt; SRR1576860     3  0.4191     0.9927 0.052 0.028 0.824 0.084 0.012
#&gt; SRR1576861     3  0.4191     0.9927 0.052 0.028 0.824 0.084 0.012
#&gt; SRR1576862     3  0.4191     0.9927 0.052 0.028 0.824 0.084 0.012
#&gt; SRR1576863     5  0.5678     0.4643 0.028 0.200 0.008 0.076 0.688
#&gt; SRR1576864     5  0.5678     0.4643 0.028 0.200 0.008 0.076 0.688
#&gt; SRR1576865     5  0.5678     0.4643 0.028 0.200 0.008 0.076 0.688
#&gt; SRR1576866     5  0.5678     0.4643 0.028 0.200 0.008 0.076 0.688
#&gt; SRR1576867     1  0.0727     0.9464 0.980 0.012 0.004 0.000 0.004
#&gt; SRR1576868     1  0.0727     0.9464 0.980 0.012 0.004 0.000 0.004
#&gt; SRR1576869     1  0.0727     0.9464 0.980 0.012 0.004 0.000 0.004
#&gt; SRR1576870     1  0.0566     0.9464 0.984 0.012 0.004 0.000 0.000
#&gt; SRR1576871     1  0.0566     0.9464 0.984 0.012 0.004 0.000 0.000
#&gt; SRR1576872     1  0.0566     0.9464 0.984 0.012 0.004 0.000 0.000
#&gt; SRR1576873     2  0.0798     0.9194 0.000 0.976 0.000 0.016 0.008
#&gt; SRR1576874     2  0.0798     0.9194 0.000 0.976 0.000 0.016 0.008
#&gt; SRR1576875     2  0.0798     0.9194 0.000 0.976 0.000 0.016 0.008
#&gt; SRR1576876     2  0.0798     0.9194 0.000 0.976 0.000 0.016 0.008
#&gt; SRR1576877     1  0.3976     0.8917 0.844 0.020 0.036 0.044 0.056
#&gt; SRR1576878     1  0.3976     0.8917 0.844 0.020 0.036 0.044 0.056
#&gt; SRR1576879     1  0.3985     0.8917 0.844 0.020 0.040 0.044 0.052
#&gt; SRR1576880     4  0.2497     0.7944 0.004 0.112 0.004 0.880 0.000
#&gt; SRR1576881     4  0.2497     0.7944 0.004 0.112 0.004 0.880 0.000
#&gt; SRR1576882     4  0.2497     0.7944 0.004 0.112 0.004 0.880 0.000
#&gt; SRR1576883     2  0.3722     0.8502 0.000 0.828 0.020 0.032 0.120
#&gt; SRR1576884     2  0.3722     0.8502 0.000 0.828 0.020 0.032 0.120
#&gt; SRR1576885     2  0.3722     0.8502 0.000 0.828 0.020 0.032 0.120
#&gt; SRR1576886     2  0.3722     0.8502 0.000 0.828 0.020 0.032 0.120
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0779      0.831 0.000 0.976 0.008 0.008 0.008 0.000
#&gt; SRR1576834     2  0.0779      0.831 0.000 0.976 0.008 0.008 0.008 0.000
#&gt; SRR1576835     2  0.0779      0.831 0.000 0.976 0.008 0.008 0.008 0.000
#&gt; SRR1576836     2  0.0779      0.831 0.000 0.976 0.008 0.008 0.008 0.000
#&gt; SRR1576837     5  0.6274      0.885 0.212 0.088 0.112 0.004 0.584 0.000
#&gt; SRR1576838     5  0.6274      0.885 0.212 0.088 0.112 0.004 0.584 0.000
#&gt; SRR1576839     5  0.6274      0.885 0.212 0.088 0.112 0.004 0.584 0.000
#&gt; SRR1576840     5  0.6274      0.885 0.212 0.088 0.112 0.004 0.584 0.000
#&gt; SRR1576841     4  0.6986      0.225 0.004 0.012 0.284 0.492 0.112 0.096
#&gt; SRR1576842     4  0.6986      0.225 0.004 0.012 0.284 0.492 0.112 0.096
#&gt; SRR1576843     4  0.6986      0.225 0.004 0.012 0.284 0.492 0.112 0.096
#&gt; SRR1576844     4  0.1851      0.734 0.000 0.036 0.000 0.928 0.012 0.024
#&gt; SRR1576845     4  0.1851      0.734 0.000 0.036 0.000 0.928 0.012 0.024
#&gt; SRR1576846     4  0.1851      0.734 0.000 0.036 0.000 0.928 0.012 0.024
#&gt; SRR1576847     4  0.4450      0.716 0.000 0.028 0.016 0.688 0.004 0.264
#&gt; SRR1576848     4  0.4450      0.716 0.000 0.028 0.016 0.688 0.004 0.264
#&gt; SRR1576849     4  0.4450      0.716 0.000 0.028 0.016 0.688 0.004 0.264
#&gt; SRR1576850     4  0.4233      0.696 0.000 0.004 0.016 0.668 0.008 0.304
#&gt; SRR1576851     4  0.4233      0.696 0.000 0.004 0.016 0.668 0.008 0.304
#&gt; SRR1576852     4  0.4233      0.696 0.000 0.004 0.016 0.668 0.008 0.304
#&gt; SRR1576853     5  0.6449      0.875 0.200 0.052 0.076 0.004 0.612 0.056
#&gt; SRR1576854     5  0.6449      0.875 0.200 0.052 0.076 0.004 0.612 0.056
#&gt; SRR1576855     5  0.6449      0.875 0.200 0.052 0.076 0.004 0.612 0.056
#&gt; SRR1576856     5  0.6449      0.875 0.200 0.052 0.076 0.004 0.612 0.056
#&gt; SRR1576857     3  0.4044      0.969 0.040 0.008 0.824 0.060 0.028 0.040
#&gt; SRR1576858     3  0.4044      0.969 0.040 0.008 0.824 0.060 0.028 0.040
#&gt; SRR1576859     3  0.4044      0.969 0.040 0.008 0.824 0.060 0.028 0.040
#&gt; SRR1576860     3  0.2527      0.969 0.040 0.008 0.892 0.056 0.004 0.000
#&gt; SRR1576861     3  0.2527      0.969 0.040 0.008 0.892 0.056 0.004 0.000
#&gt; SRR1576862     3  0.2527      0.969 0.040 0.008 0.892 0.056 0.004 0.000
#&gt; SRR1576863     6  0.6743      0.994 0.012 0.152 0.004 0.036 0.348 0.448
#&gt; SRR1576864     6  0.6743      0.994 0.012 0.152 0.004 0.036 0.348 0.448
#&gt; SRR1576865     6  0.6603      0.994 0.012 0.152 0.000 0.036 0.340 0.460
#&gt; SRR1576866     6  0.6603      0.994 0.012 0.152 0.000 0.036 0.340 0.460
#&gt; SRR1576867     1  0.0146      0.913 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576868     1  0.0146      0.913 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576869     1  0.0146      0.913 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576870     1  0.0982      0.913 0.968 0.004 0.004 0.004 0.020 0.000
#&gt; SRR1576871     1  0.0982      0.913 0.968 0.004 0.004 0.004 0.020 0.000
#&gt; SRR1576872     1  0.0982      0.913 0.968 0.004 0.004 0.004 0.020 0.000
#&gt; SRR1576873     2  0.2739      0.817 0.000 0.888 0.044 0.008 0.036 0.024
#&gt; SRR1576874     2  0.2739      0.817 0.000 0.888 0.044 0.008 0.036 0.024
#&gt; SRR1576875     2  0.2739      0.817 0.000 0.888 0.044 0.008 0.036 0.024
#&gt; SRR1576876     2  0.2739      0.817 0.000 0.888 0.044 0.008 0.036 0.024
#&gt; SRR1576877     1  0.4157      0.838 0.784 0.000 0.012 0.016 0.064 0.124
#&gt; SRR1576878     1  0.4205      0.838 0.784 0.000 0.016 0.016 0.064 0.120
#&gt; SRR1576879     1  0.4157      0.838 0.784 0.000 0.012 0.016 0.064 0.124
#&gt; SRR1576880     4  0.0972      0.741 0.000 0.028 0.000 0.964 0.008 0.000
#&gt; SRR1576881     4  0.0972      0.741 0.000 0.028 0.000 0.964 0.008 0.000
#&gt; SRR1576882     4  0.0972      0.741 0.000 0.028 0.000 0.964 0.008 0.000
#&gt; SRR1576883     2  0.5497      0.678 0.004 0.704 0.028 0.040 0.092 0.132
#&gt; SRR1576884     2  0.5497      0.678 0.004 0.704 0.028 0.040 0.092 0.132
#&gt; SRR1576885     2  0.5497      0.678 0.004 0.704 0.028 0.040 0.092 0.132
#&gt; SRR1576886     2  0.5497      0.678 0.004 0.704 0.028 0.040 0.092 0.132
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.500           0.888       0.913         0.5046 0.491   0.491
#> 3 3 0.866           0.901       0.945         0.3260 0.683   0.441
#> 4 4 0.855           0.941       0.948         0.1208 0.925   0.768
#> 5 5 0.908           0.940       0.955         0.0758 0.894   0.610
#> 6 6 0.932           0.935       0.949         0.0354 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.689      0.899 0.184 0.816
#&gt; SRR1576834     2   0.689      0.899 0.184 0.816
#&gt; SRR1576835     2   0.689      0.899 0.184 0.816
#&gt; SRR1576836     2   0.689      0.899 0.184 0.816
#&gt; SRR1576837     1   0.000      0.919 1.000 0.000
#&gt; SRR1576838     1   0.000      0.919 1.000 0.000
#&gt; SRR1576839     1   0.000      0.919 1.000 0.000
#&gt; SRR1576840     1   0.000      0.919 1.000 0.000
#&gt; SRR1576841     1   0.722      0.837 0.800 0.200
#&gt; SRR1576842     1   0.722      0.837 0.800 0.200
#&gt; SRR1576843     1   0.722      0.837 0.800 0.200
#&gt; SRR1576844     2   0.000      0.869 0.000 1.000
#&gt; SRR1576845     2   0.000      0.869 0.000 1.000
#&gt; SRR1576846     2   0.000      0.869 0.000 1.000
#&gt; SRR1576847     2   0.000      0.869 0.000 1.000
#&gt; SRR1576848     2   0.000      0.869 0.000 1.000
#&gt; SRR1576849     2   0.000      0.869 0.000 1.000
#&gt; SRR1576850     2   0.000      0.869 0.000 1.000
#&gt; SRR1576851     2   0.000      0.869 0.000 1.000
#&gt; SRR1576852     2   0.000      0.869 0.000 1.000
#&gt; SRR1576853     1   0.000      0.919 1.000 0.000
#&gt; SRR1576854     1   0.000      0.919 1.000 0.000
#&gt; SRR1576855     1   0.000      0.919 1.000 0.000
#&gt; SRR1576856     1   0.000      0.919 1.000 0.000
#&gt; SRR1576857     1   0.697      0.843 0.812 0.188
#&gt; SRR1576858     1   0.697      0.843 0.812 0.188
#&gt; SRR1576859     1   0.697      0.843 0.812 0.188
#&gt; SRR1576860     1   0.697      0.843 0.812 0.188
#&gt; SRR1576861     1   0.697      0.843 0.812 0.188
#&gt; SRR1576862     1   0.697      0.843 0.812 0.188
#&gt; SRR1576863     2   0.697      0.896 0.188 0.812
#&gt; SRR1576864     2   0.697      0.896 0.188 0.812
#&gt; SRR1576865     2   0.697      0.896 0.188 0.812
#&gt; SRR1576866     2   0.697      0.896 0.188 0.812
#&gt; SRR1576867     1   0.000      0.919 1.000 0.000
#&gt; SRR1576868     1   0.000      0.919 1.000 0.000
#&gt; SRR1576869     1   0.000      0.919 1.000 0.000
#&gt; SRR1576870     1   0.000      0.919 1.000 0.000
#&gt; SRR1576871     1   0.000      0.919 1.000 0.000
#&gt; SRR1576872     1   0.000      0.919 1.000 0.000
#&gt; SRR1576873     2   0.689      0.899 0.184 0.816
#&gt; SRR1576874     2   0.689      0.899 0.184 0.816
#&gt; SRR1576875     2   0.689      0.899 0.184 0.816
#&gt; SRR1576876     2   0.689      0.899 0.184 0.816
#&gt; SRR1576877     1   0.000      0.919 1.000 0.000
#&gt; SRR1576878     1   0.000      0.919 1.000 0.000
#&gt; SRR1576879     1   0.000      0.919 1.000 0.000
#&gt; SRR1576880     2   0.000      0.869 0.000 1.000
#&gt; SRR1576881     2   0.000      0.869 0.000 1.000
#&gt; SRR1576882     2   0.000      0.869 0.000 1.000
#&gt; SRR1576883     2   0.689      0.899 0.184 0.816
#&gt; SRR1576884     2   0.689      0.899 0.184 0.816
#&gt; SRR1576885     2   0.689      0.899 0.184 0.816
#&gt; SRR1576886     2   0.689      0.899 0.184 0.816
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576837     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576838     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576839     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576840     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576841     3  0.0237      0.843 0.004 0.00 0.996
#&gt; SRR1576842     3  0.0237      0.843 0.004 0.00 0.996
#&gt; SRR1576843     3  0.0237      0.843 0.004 0.00 0.996
#&gt; SRR1576844     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576845     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576846     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576847     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576848     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576849     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576850     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576851     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576852     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576853     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576854     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576855     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576856     1  0.0424      0.995 0.992 0.00 0.008
#&gt; SRR1576857     3  0.6095      0.485 0.392 0.00 0.608
#&gt; SRR1576858     3  0.6095      0.485 0.392 0.00 0.608
#&gt; SRR1576859     3  0.6095      0.485 0.392 0.00 0.608
#&gt; SRR1576860     3  0.6095      0.485 0.392 0.00 0.608
#&gt; SRR1576861     3  0.6095      0.485 0.392 0.00 0.608
#&gt; SRR1576862     3  0.6095      0.485 0.392 0.00 0.608
#&gt; SRR1576863     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576864     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576865     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576866     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576867     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576868     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576869     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576870     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576871     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576872     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576877     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576878     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576879     1  0.0000      0.996 1.000 0.00 0.000
#&gt; SRR1576880     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576881     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576882     3  0.1529      0.855 0.000 0.04 0.960
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.00 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.00 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576834     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576835     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576836     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576837     1  0.3529      0.872 0.836 0.012 0.152 0.000
#&gt; SRR1576838     1  0.3529      0.872 0.836 0.012 0.152 0.000
#&gt; SRR1576839     1  0.3577      0.869 0.832 0.012 0.156 0.000
#&gt; SRR1576840     1  0.3577      0.869 0.832 0.012 0.156 0.000
#&gt; SRR1576841     3  0.2408      0.926 0.000 0.000 0.896 0.104
#&gt; SRR1576842     3  0.2408      0.926 0.000 0.000 0.896 0.104
#&gt; SRR1576843     3  0.2408      0.926 0.000 0.000 0.896 0.104
#&gt; SRR1576844     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576845     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576846     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576847     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576848     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576849     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576850     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576851     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576852     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576853     1  0.3577      0.870 0.832 0.012 0.156 0.000
#&gt; SRR1576854     1  0.3577      0.870 0.832 0.012 0.156 0.000
#&gt; SRR1576855     1  0.3428      0.875 0.844 0.012 0.144 0.000
#&gt; SRR1576856     1  0.3428      0.875 0.844 0.012 0.144 0.000
#&gt; SRR1576857     3  0.0707      0.964 0.000 0.000 0.980 0.020
#&gt; SRR1576858     3  0.0707      0.964 0.000 0.000 0.980 0.020
#&gt; SRR1576859     3  0.0707      0.964 0.000 0.000 0.980 0.020
#&gt; SRR1576860     3  0.0707      0.964 0.000 0.000 0.980 0.020
#&gt; SRR1576861     3  0.0707      0.964 0.000 0.000 0.980 0.020
#&gt; SRR1576862     3  0.0707      0.964 0.000 0.000 0.980 0.020
#&gt; SRR1576863     2  0.3599      0.895 0.040 0.876 0.020 0.064
#&gt; SRR1576864     2  0.3599      0.895 0.040 0.876 0.020 0.064
#&gt; SRR1576865     2  0.3599      0.895 0.040 0.876 0.020 0.064
#&gt; SRR1576866     2  0.3599      0.895 0.040 0.876 0.020 0.064
#&gt; SRR1576867     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576868     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576869     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576870     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576871     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576872     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576873     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576874     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576875     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576876     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576877     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576878     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576879     1  0.1389      0.897 0.952 0.000 0.048 0.000
#&gt; SRR1576880     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576881     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576882     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576883     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576884     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576885     2  0.0469      0.967 0.000 0.988 0.000 0.012
#&gt; SRR1576886     2  0.0469      0.967 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.3492      0.797 0.188 0.000 0.016 0.000 0.796
#&gt; SRR1576838     5  0.3492      0.797 0.188 0.000 0.016 0.000 0.796
#&gt; SRR1576839     5  0.3492      0.797 0.188 0.000 0.016 0.000 0.796
#&gt; SRR1576840     5  0.3492      0.797 0.188 0.000 0.016 0.000 0.796
#&gt; SRR1576841     3  0.1608      0.934 0.000 0.000 0.928 0.072 0.000
#&gt; SRR1576842     3  0.1608      0.934 0.000 0.000 0.928 0.072 0.000
#&gt; SRR1576843     3  0.1608      0.934 0.000 0.000 0.928 0.072 0.000
#&gt; SRR1576844     4  0.0451      0.995 0.000 0.000 0.008 0.988 0.004
#&gt; SRR1576845     4  0.0451      0.995 0.000 0.000 0.008 0.988 0.004
#&gt; SRR1576846     4  0.0451      0.995 0.000 0.000 0.008 0.988 0.004
#&gt; SRR1576847     4  0.0000      0.995 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576848     4  0.0000      0.995 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576849     4  0.0000      0.995 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576850     4  0.0000      0.995 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576851     4  0.0000      0.995 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576852     4  0.0000      0.995 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576853     5  0.1942      0.842 0.068 0.000 0.012 0.000 0.920
#&gt; SRR1576854     5  0.1942      0.842 0.068 0.000 0.012 0.000 0.920
#&gt; SRR1576855     5  0.1942      0.842 0.068 0.000 0.012 0.000 0.920
#&gt; SRR1576856     5  0.1942      0.842 0.068 0.000 0.012 0.000 0.920
#&gt; SRR1576857     3  0.0324      0.967 0.004 0.000 0.992 0.000 0.004
#&gt; SRR1576858     3  0.0324      0.967 0.004 0.000 0.992 0.000 0.004
#&gt; SRR1576859     3  0.0324      0.967 0.004 0.000 0.992 0.000 0.004
#&gt; SRR1576860     3  0.0324      0.967 0.004 0.000 0.992 0.000 0.004
#&gt; SRR1576861     3  0.0324      0.967 0.004 0.000 0.992 0.000 0.004
#&gt; SRR1576862     3  0.0324      0.967 0.004 0.000 0.992 0.000 0.004
#&gt; SRR1576863     5  0.3171      0.746 0.000 0.176 0.000 0.008 0.816
#&gt; SRR1576864     5  0.3171      0.746 0.000 0.176 0.000 0.008 0.816
#&gt; SRR1576865     5  0.3171      0.746 0.000 0.176 0.000 0.008 0.816
#&gt; SRR1576866     5  0.3171      0.746 0.000 0.176 0.000 0.008 0.816
#&gt; SRR1576867     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.981 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0451      0.995 0.000 0.000 0.008 0.988 0.004
#&gt; SRR1576881     4  0.0451      0.995 0.000 0.000 0.008 0.988 0.004
#&gt; SRR1576882     4  0.0451      0.995 0.000 0.000 0.008 0.988 0.004
#&gt; SRR1576883     2  0.1341      0.961 0.000 0.944 0.000 0.000 0.056
#&gt; SRR1576884     2  0.1341      0.961 0.000 0.944 0.000 0.000 0.056
#&gt; SRR1576885     2  0.1341      0.961 0.000 0.944 0.000 0.000 0.056
#&gt; SRR1576886     2  0.1341      0.961 0.000 0.944 0.000 0.000 0.056
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0000      0.895 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.895 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.895 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.895 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.0547      0.913 0.020 0.000 0.000 0.000 0.980 0.000
#&gt; SRR1576838     5  0.0547      0.913 0.020 0.000 0.000 0.000 0.980 0.000
#&gt; SRR1576839     5  0.0547      0.913 0.020 0.000 0.000 0.000 0.980 0.000
#&gt; SRR1576840     5  0.0547      0.913 0.020 0.000 0.000 0.000 0.980 0.000
#&gt; SRR1576841     3  0.2376      0.905 0.000 0.000 0.888 0.068 0.000 0.044
#&gt; SRR1576842     3  0.2376      0.905 0.000 0.000 0.888 0.068 0.000 0.044
#&gt; SRR1576843     3  0.2376      0.905 0.000 0.000 0.888 0.068 0.000 0.044
#&gt; SRR1576844     4  0.1349      0.971 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576845     4  0.1349      0.971 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576846     4  0.1349      0.971 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576847     4  0.0000      0.971 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576848     4  0.0000      0.971 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576849     4  0.0000      0.971 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576850     4  0.0000      0.971 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576851     4  0.0000      0.971 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576852     4  0.0000      0.971 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576853     5  0.2191      0.907 0.004 0.000 0.000 0.000 0.876 0.120
#&gt; SRR1576854     5  0.2191      0.907 0.004 0.000 0.000 0.000 0.876 0.120
#&gt; SRR1576855     5  0.2191      0.907 0.004 0.000 0.000 0.000 0.876 0.120
#&gt; SRR1576856     5  0.2191      0.907 0.004 0.000 0.000 0.000 0.876 0.120
#&gt; SRR1576857     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576858     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576859     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576860     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576861     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576862     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576863     6  0.2039      1.000 0.000 0.020 0.000 0.000 0.076 0.904
#&gt; SRR1576864     6  0.2039      1.000 0.000 0.020 0.000 0.000 0.076 0.904
#&gt; SRR1576865     6  0.2039      1.000 0.000 0.020 0.000 0.000 0.076 0.904
#&gt; SRR1576866     6  0.2039      1.000 0.000 0.020 0.000 0.000 0.076 0.904
#&gt; SRR1576867     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576868     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576869     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576870     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576871     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576872     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1576873     2  0.0291      0.894 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; SRR1576874     2  0.0291      0.894 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; SRR1576875     2  0.0291      0.894 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; SRR1576876     2  0.0291      0.894 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; SRR1576877     1  0.0146      0.995 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1576878     1  0.0146      0.995 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1576879     1  0.0146      0.995 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1576880     4  0.1349      0.971 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576881     4  0.1349      0.971 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576882     4  0.1349      0.971 0.000 0.000 0.004 0.940 0.000 0.056
#&gt; SRR1576883     2  0.3265      0.752 0.000 0.748 0.000 0.000 0.004 0.248
#&gt; SRR1576884     2  0.3265      0.752 0.000 0.748 0.000 0.000 0.004 0.248
#&gt; SRR1576885     2  0.3265      0.752 0.000 0.748 0.000 0.000 0.004 0.248
#&gt; SRR1576886     2  0.3265      0.752 0.000 0.748 0.000 0.000 0.004 0.248
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.397           0.627       0.831         0.4968 0.497   0.497
#> 3 3 0.479           0.651       0.776         0.2055 0.623   0.433
#> 4 4 0.738           0.844       0.905         0.2276 0.772   0.516
#> 5 5 1.000           0.956       0.975         0.0974 0.885   0.592
#> 6 6 1.000           0.959       0.980         0.0257 0.948   0.748
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.978      0.574 0.412 0.588
#&gt; SRR1576834     2   0.978      0.574 0.412 0.588
#&gt; SRR1576835     2   0.952      0.613 0.372 0.628
#&gt; SRR1576836     2   0.963      0.601 0.388 0.612
#&gt; SRR1576837     1   0.000      0.769 1.000 0.000
#&gt; SRR1576838     1   0.000      0.769 1.000 0.000
#&gt; SRR1576839     1   0.000      0.769 1.000 0.000
#&gt; SRR1576840     1   0.000      0.769 1.000 0.000
#&gt; SRR1576841     1   0.988      0.432 0.564 0.436
#&gt; SRR1576842     1   0.988      0.432 0.564 0.436
#&gt; SRR1576843     1   0.988      0.432 0.564 0.436
#&gt; SRR1576844     2   0.000      0.737 0.000 1.000
#&gt; SRR1576845     2   0.000      0.737 0.000 1.000
#&gt; SRR1576846     2   0.000      0.737 0.000 1.000
#&gt; SRR1576847     2   0.000      0.737 0.000 1.000
#&gt; SRR1576848     2   0.000      0.737 0.000 1.000
#&gt; SRR1576849     2   0.000      0.737 0.000 1.000
#&gt; SRR1576850     2   0.000      0.737 0.000 1.000
#&gt; SRR1576851     2   0.000      0.737 0.000 1.000
#&gt; SRR1576852     2   0.000      0.737 0.000 1.000
#&gt; SRR1576853     1   0.000      0.769 1.000 0.000
#&gt; SRR1576854     1   0.000      0.769 1.000 0.000
#&gt; SRR1576855     1   0.000      0.769 1.000 0.000
#&gt; SRR1576856     1   0.000      0.769 1.000 0.000
#&gt; SRR1576857     1   0.904      0.575 0.680 0.320
#&gt; SRR1576858     1   0.904      0.575 0.680 0.320
#&gt; SRR1576859     1   0.904      0.575 0.680 0.320
#&gt; SRR1576860     1   0.900      0.578 0.684 0.316
#&gt; SRR1576861     1   0.904      0.575 0.680 0.320
#&gt; SRR1576862     1   0.871      0.595 0.708 0.292
#&gt; SRR1576863     1   0.861      0.300 0.716 0.284
#&gt; SRR1576864     1   0.921      0.142 0.664 0.336
#&gt; SRR1576865     1   0.993     -0.291 0.548 0.452
#&gt; SRR1576866     1   0.998     -0.358 0.524 0.476
#&gt; SRR1576867     1   0.000      0.769 1.000 0.000
#&gt; SRR1576868     1   0.000      0.769 1.000 0.000
#&gt; SRR1576869     1   0.000      0.769 1.000 0.000
#&gt; SRR1576870     1   0.000      0.769 1.000 0.000
#&gt; SRR1576871     1   0.000      0.769 1.000 0.000
#&gt; SRR1576872     1   0.000      0.769 1.000 0.000
#&gt; SRR1576873     2   0.975      0.579 0.408 0.592
#&gt; SRR1576874     2   0.975      0.579 0.408 0.592
#&gt; SRR1576875     2   0.969      0.593 0.396 0.604
#&gt; SRR1576876     2   0.939      0.623 0.356 0.644
#&gt; SRR1576877     1   0.000      0.769 1.000 0.000
#&gt; SRR1576878     1   0.000      0.769 1.000 0.000
#&gt; SRR1576879     1   0.000      0.769 1.000 0.000
#&gt; SRR1576880     2   0.000      0.737 0.000 1.000
#&gt; SRR1576881     2   0.000      0.737 0.000 1.000
#&gt; SRR1576882     2   0.000      0.737 0.000 1.000
#&gt; SRR1576883     2   0.891      0.660 0.308 0.692
#&gt; SRR1576884     2   0.881      0.664 0.300 0.700
#&gt; SRR1576885     2   0.900      0.654 0.316 0.684
#&gt; SRR1576886     2   0.900      0.654 0.316 0.684
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576834     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576835     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576836     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576837     2  0.9553      0.447 0.244 0.484 0.272
#&gt; SRR1576838     2  0.9528      0.450 0.240 0.488 0.272
#&gt; SRR1576839     2  0.7672      0.441 0.044 0.488 0.468
#&gt; SRR1576840     2  0.7672      0.441 0.044 0.488 0.468
#&gt; SRR1576841     3  0.0424      0.989 0.000 0.008 0.992
#&gt; SRR1576842     3  0.0424      0.989 0.000 0.008 0.992
#&gt; SRR1576843     3  0.0424      0.989 0.000 0.008 0.992
#&gt; SRR1576844     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576845     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576846     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576847     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576848     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576849     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576850     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576851     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576852     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576853     2  0.9910      0.344 0.344 0.384 0.272
#&gt; SRR1576854     2  0.9919      0.326 0.356 0.372 0.272
#&gt; SRR1576855     1  0.7911      0.407 0.632 0.096 0.272
#&gt; SRR1576856     1  0.7841      0.414 0.636 0.092 0.272
#&gt; SRR1576857     3  0.0000      0.994 0.000 0.000 1.000
#&gt; SRR1576858     3  0.0000      0.994 0.000 0.000 1.000
#&gt; SRR1576859     3  0.0000      0.994 0.000 0.000 1.000
#&gt; SRR1576860     3  0.0000      0.994 0.000 0.000 1.000
#&gt; SRR1576861     3  0.0000      0.994 0.000 0.000 1.000
#&gt; SRR1576862     3  0.0000      0.994 0.000 0.000 1.000
#&gt; SRR1576863     2  0.1999      0.570 0.036 0.952 0.012
#&gt; SRR1576864     2  0.1832      0.570 0.036 0.956 0.008
#&gt; SRR1576865     2  0.1832      0.570 0.036 0.956 0.008
#&gt; SRR1576866     2  0.1832      0.570 0.036 0.956 0.008
#&gt; SRR1576867     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576873     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576874     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576875     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576876     2  0.5327      0.575 0.000 0.728 0.272
#&gt; SRR1576877     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.905 1.000 0.000 0.000
#&gt; SRR1576880     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576881     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576882     2  0.5733      0.469 0.000 0.676 0.324
#&gt; SRR1576883     2  0.5254      0.576 0.000 0.736 0.264
#&gt; SRR1576884     2  0.5254      0.576 0.000 0.736 0.264
#&gt; SRR1576885     2  0.5254      0.576 0.000 0.736 0.264
#&gt; SRR1576886     2  0.5254      0.576 0.000 0.736 0.264
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576834     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576835     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576836     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576837     2  0.4599      0.602 0.248 0.736 0.016 0.000
#&gt; SRR1576838     2  0.4599      0.602 0.248 0.736 0.016 0.000
#&gt; SRR1576839     2  0.5831      0.693 0.112 0.736 0.016 0.136
#&gt; SRR1576840     2  0.5831      0.693 0.112 0.736 0.016 0.136
#&gt; SRR1576841     3  0.4072      0.763 0.000 0.000 0.748 0.252
#&gt; SRR1576842     3  0.4103      0.759 0.000 0.000 0.744 0.256
#&gt; SRR1576843     3  0.4103      0.759 0.000 0.000 0.744 0.256
#&gt; SRR1576844     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576845     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576846     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576847     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576848     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576849     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576850     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576851     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576852     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576853     1  0.5513      0.367 0.596 0.384 0.016 0.004
#&gt; SRR1576854     1  0.5458      0.410 0.612 0.368 0.016 0.004
#&gt; SRR1576855     1  0.3280      0.818 0.860 0.124 0.016 0.000
#&gt; SRR1576856     1  0.3224      0.822 0.864 0.120 0.016 0.000
#&gt; SRR1576857     3  0.0592      0.904 0.000 0.000 0.984 0.016
#&gt; SRR1576858     3  0.0592      0.904 0.000 0.000 0.984 0.016
#&gt; SRR1576859     3  0.0592      0.904 0.000 0.000 0.984 0.016
#&gt; SRR1576860     3  0.0592      0.904 0.000 0.000 0.984 0.016
#&gt; SRR1576861     3  0.0592      0.904 0.000 0.000 0.984 0.016
#&gt; SRR1576862     3  0.0592      0.904 0.000 0.000 0.984 0.016
#&gt; SRR1576863     2  0.6577      0.641 0.108 0.660 0.016 0.216
#&gt; SRR1576864     2  0.6577      0.641 0.108 0.660 0.016 0.216
#&gt; SRR1576865     2  0.6577      0.641 0.108 0.660 0.016 0.216
#&gt; SRR1576866     2  0.6577      0.641 0.108 0.660 0.016 0.216
#&gt; SRR1576867     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576874     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576875     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576876     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576877     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.897 1.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576881     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576882     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576883     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576884     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576885     2  0.1637      0.852 0.000 0.940 0.000 0.060
#&gt; SRR1576886     2  0.1637      0.852 0.000 0.940 0.000 0.060
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5
#&gt; SRR1576833     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576837     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576838     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576839     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576840     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576841     3   0.489      0.660  0 0.000 0.660 0.288 0.052
#&gt; SRR1576842     3   0.493      0.651  0 0.000 0.652 0.296 0.052
#&gt; SRR1576843     3   0.493      0.651  0 0.000 0.652 0.296 0.052
#&gt; SRR1576844     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576845     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576846     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576847     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576848     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576849     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576850     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576851     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576852     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576853     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576854     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576855     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576856     5   0.000      0.967  0 0.000 0.000 0.000 1.000
#&gt; SRR1576857     3   0.000      0.865  0 0.000 1.000 0.000 0.000
#&gt; SRR1576858     3   0.000      0.865  0 0.000 1.000 0.000 0.000
#&gt; SRR1576859     3   0.000      0.865  0 0.000 1.000 0.000 0.000
#&gt; SRR1576860     3   0.000      0.865  0 0.000 1.000 0.000 0.000
#&gt; SRR1576861     3   0.000      0.865  0 0.000 1.000 0.000 0.000
#&gt; SRR1576862     3   0.000      0.865  0 0.000 1.000 0.000 0.000
#&gt; SRR1576863     5   0.167      0.934  0 0.076 0.000 0.000 0.924
#&gt; SRR1576864     5   0.167      0.934  0 0.076 0.000 0.000 0.924
#&gt; SRR1576865     5   0.167      0.934  0 0.076 0.000 0.000 0.924
#&gt; SRR1576866     5   0.167      0.934  0 0.076 0.000 0.000 0.924
#&gt; SRR1576867     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1   0.000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576881     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576882     4   0.000      1.000  0 0.000 0.000 1.000 0.000
#&gt; SRR1576883     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576884     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576885     2   0.000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1576886     2   0.000      1.000  0 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2    p3    p4    p5 p6
#&gt; SRR1576833     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576834     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576835     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576836     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576837     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576838     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576839     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576840     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576841     5   0.475      0.612  0  0 0.080 0.288 0.632  0
#&gt; SRR1576842     5   0.469      0.606  0  0 0.072 0.296 0.632  0
#&gt; SRR1576843     5   0.469      0.606  0  0 0.072 0.296 0.632  0
#&gt; SRR1576844     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576845     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576846     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576847     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576848     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576849     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576850     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576851     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576852     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576853     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576854     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576855     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576856     5   0.000      0.868  0  0 0.000 0.000 1.000  0
#&gt; SRR1576857     3   0.000      1.000  0  0 1.000 0.000 0.000  0
#&gt; SRR1576858     3   0.000      1.000  0  0 1.000 0.000 0.000  0
#&gt; SRR1576859     3   0.000      1.000  0  0 1.000 0.000 0.000  0
#&gt; SRR1576860     3   0.000      1.000  0  0 1.000 0.000 0.000  0
#&gt; SRR1576861     3   0.000      1.000  0  0 1.000 0.000 0.000  0
#&gt; SRR1576862     3   0.000      1.000  0  0 1.000 0.000 0.000  0
#&gt; SRR1576863     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576864     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576865     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576866     6   0.000      1.000  0  0 0.000 0.000 0.000  1
#&gt; SRR1576867     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576868     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576869     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576870     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576871     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576872     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576873     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576874     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576875     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576876     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576877     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576878     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576879     1   0.000      1.000  1  0 0.000 0.000 0.000  0
#&gt; SRR1576880     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576881     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576882     4   0.000      1.000  0  0 0.000 1.000 0.000  0
#&gt; SRR1576883     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576884     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576885     2   0.000      1.000  0  1 0.000 0.000 0.000  0
#&gt; SRR1576886     2   0.000      1.000  0  1 0.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.305           0.735       0.764         0.3845 0.648   0.648
#> 3 3 0.291           0.572       0.687         0.6068 0.748   0.612
#> 4 4 0.438           0.634       0.797         0.1662 0.728   0.413
#> 5 5 0.871           0.809       0.891         0.1131 0.885   0.592
#> 6 6 0.844           0.798       0.866         0.0351 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.000      0.978 0.000 1.000
#&gt; SRR1576834     2   0.000      0.978 0.000 1.000
#&gt; SRR1576835     2   0.000      0.978 0.000 1.000
#&gt; SRR1576836     2   0.000      0.978 0.000 1.000
#&gt; SRR1576837     1   0.981      0.721 0.580 0.420
#&gt; SRR1576838     1   0.981      0.721 0.580 0.420
#&gt; SRR1576839     1   0.981      0.721 0.580 0.420
#&gt; SRR1576840     1   0.981      0.721 0.580 0.420
#&gt; SRR1576841     1   0.963      0.731 0.612 0.388
#&gt; SRR1576842     1   0.963      0.731 0.612 0.388
#&gt; SRR1576843     1   0.963      0.731 0.612 0.388
#&gt; SRR1576844     1   0.987      0.717 0.568 0.432
#&gt; SRR1576845     1   0.987      0.717 0.568 0.432
#&gt; SRR1576846     1   0.987      0.717 0.568 0.432
#&gt; SRR1576847     1   0.987      0.717 0.568 0.432
#&gt; SRR1576848     1   0.987      0.717 0.568 0.432
#&gt; SRR1576849     1   0.987      0.717 0.568 0.432
#&gt; SRR1576850     1   0.971      0.723 0.600 0.400
#&gt; SRR1576851     1   0.971      0.723 0.600 0.400
#&gt; SRR1576852     1   0.971      0.723 0.600 0.400
#&gt; SRR1576853     1   0.929      0.716 0.656 0.344
#&gt; SRR1576854     1   0.929      0.716 0.656 0.344
#&gt; SRR1576855     1   0.929      0.716 0.656 0.344
#&gt; SRR1576856     1   0.929      0.716 0.656 0.344
#&gt; SRR1576857     1   0.871      0.610 0.708 0.292
#&gt; SRR1576858     1   0.871      0.610 0.708 0.292
#&gt; SRR1576859     1   0.871      0.610 0.708 0.292
#&gt; SRR1576860     1   0.871      0.610 0.708 0.292
#&gt; SRR1576861     1   0.871      0.610 0.708 0.292
#&gt; SRR1576862     1   0.871      0.610 0.708 0.292
#&gt; SRR1576863     1   0.987      0.645 0.568 0.432
#&gt; SRR1576864     1   0.987      0.645 0.568 0.432
#&gt; SRR1576865     1   0.987      0.645 0.568 0.432
#&gt; SRR1576866     1   0.987      0.645 0.568 0.432
#&gt; SRR1576867     1   0.163      0.585 0.976 0.024
#&gt; SRR1576868     1   0.163      0.585 0.976 0.024
#&gt; SRR1576869     1   0.163      0.585 0.976 0.024
#&gt; SRR1576870     1   0.163      0.585 0.976 0.024
#&gt; SRR1576871     1   0.163      0.585 0.976 0.024
#&gt; SRR1576872     1   0.163      0.585 0.976 0.024
#&gt; SRR1576873     2   0.000      0.978 0.000 1.000
#&gt; SRR1576874     2   0.000      0.978 0.000 1.000
#&gt; SRR1576875     2   0.000      0.978 0.000 1.000
#&gt; SRR1576876     2   0.000      0.978 0.000 1.000
#&gt; SRR1576877     1   0.278      0.585 0.952 0.048
#&gt; SRR1576878     1   0.278      0.585 0.952 0.048
#&gt; SRR1576879     1   0.278      0.585 0.952 0.048
#&gt; SRR1576880     1   0.987      0.717 0.568 0.432
#&gt; SRR1576881     1   0.987      0.717 0.568 0.432
#&gt; SRR1576882     1   0.987      0.717 0.568 0.432
#&gt; SRR1576883     2   0.204      0.955 0.032 0.968
#&gt; SRR1576884     2   0.204      0.955 0.032 0.968
#&gt; SRR1576885     2   0.204      0.955 0.032 0.968
#&gt; SRR1576886     2   0.204      0.955 0.032 0.968
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2   0.766     0.9804 0.104 0.668 0.228
#&gt; SRR1576834     2   0.766     0.9804 0.104 0.668 0.228
#&gt; SRR1576835     2   0.737     0.9672 0.104 0.696 0.200
#&gt; SRR1576836     2   0.737     0.9672 0.104 0.696 0.200
#&gt; SRR1576837     1   0.127     0.4850 0.972 0.004 0.024
#&gt; SRR1576838     1   0.127     0.4850 0.972 0.004 0.024
#&gt; SRR1576839     1   0.103     0.4852 0.976 0.000 0.024
#&gt; SRR1576840     1   0.103     0.4852 0.976 0.000 0.024
#&gt; SRR1576841     1   0.626    -0.0437 0.552 0.000 0.448
#&gt; SRR1576842     1   0.626    -0.0437 0.552 0.000 0.448
#&gt; SRR1576843     1   0.626    -0.0437 0.552 0.000 0.448
#&gt; SRR1576844     3   0.405     0.9088 0.148 0.004 0.848
#&gt; SRR1576845     3   0.405     0.9088 0.148 0.004 0.848
#&gt; SRR1576846     3   0.405     0.9088 0.148 0.004 0.848
#&gt; SRR1576847     3   0.405     0.9088 0.148 0.004 0.848
#&gt; SRR1576848     3   0.405     0.9088 0.148 0.004 0.848
#&gt; SRR1576849     3   0.405     0.9088 0.148 0.004 0.848
#&gt; SRR1576850     3   0.579     0.7197 0.332 0.000 0.668
#&gt; SRR1576851     3   0.579     0.7197 0.332 0.000 0.668
#&gt; SRR1576852     3   0.579     0.7197 0.332 0.000 0.668
#&gt; SRR1576853     1   0.236     0.4666 0.928 0.000 0.072
#&gt; SRR1576854     1   0.236     0.4666 0.928 0.000 0.072
#&gt; SRR1576855     1   0.236     0.4666 0.928 0.000 0.072
#&gt; SRR1576856     1   0.236     0.4666 0.928 0.000 0.072
#&gt; SRR1576857     1   0.614     0.0777 0.596 0.000 0.404
#&gt; SRR1576858     1   0.614     0.0777 0.596 0.000 0.404
#&gt; SRR1576859     1   0.614     0.0777 0.596 0.000 0.404
#&gt; SRR1576860     1   0.614     0.0777 0.596 0.000 0.404
#&gt; SRR1576861     1   0.614     0.0777 0.596 0.000 0.404
#&gt; SRR1576862     1   0.614     0.0777 0.596 0.000 0.404
#&gt; SRR1576863     1   0.821     0.2561 0.600 0.296 0.104
#&gt; SRR1576864     1   0.821     0.2561 0.600 0.296 0.104
#&gt; SRR1576865     1   0.821     0.2561 0.600 0.296 0.104
#&gt; SRR1576866     1   0.821     0.2561 0.600 0.296 0.104
#&gt; SRR1576867     1   0.901     0.4184 0.536 0.304 0.160
#&gt; SRR1576868     1   0.901     0.4184 0.536 0.304 0.160
#&gt; SRR1576869     1   0.901     0.4184 0.536 0.304 0.160
#&gt; SRR1576870     1   0.901     0.4184 0.536 0.304 0.160
#&gt; SRR1576871     1   0.901     0.4184 0.536 0.304 0.160
#&gt; SRR1576872     1   0.901     0.4184 0.536 0.304 0.160
#&gt; SRR1576873     2   0.737     0.9672 0.104 0.696 0.200
#&gt; SRR1576874     2   0.737     0.9672 0.104 0.696 0.200
#&gt; SRR1576875     2   0.766     0.9804 0.104 0.668 0.228
#&gt; SRR1576876     2   0.762     0.9801 0.104 0.672 0.224
#&gt; SRR1576877     1   0.920     0.4055 0.476 0.368 0.156
#&gt; SRR1576878     1   0.920     0.4055 0.476 0.368 0.156
#&gt; SRR1576879     1   0.920     0.4055 0.476 0.368 0.156
#&gt; SRR1576880     3   0.512     0.8927 0.152 0.032 0.816
#&gt; SRR1576881     3   0.512     0.8927 0.152 0.032 0.816
#&gt; SRR1576882     3   0.512     0.8927 0.152 0.032 0.816
#&gt; SRR1576883     2   0.773     0.9796 0.108 0.664 0.228
#&gt; SRR1576884     2   0.773     0.9796 0.108 0.664 0.228
#&gt; SRR1576885     2   0.773     0.9796 0.108 0.664 0.228
#&gt; SRR1576886     2   0.773     0.9796 0.108 0.664 0.228
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576834     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576835     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576836     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576837     1   0.924      0.252 0.420 0.136 0.152 0.292
#&gt; SRR1576838     1   0.924      0.252 0.420 0.136 0.152 0.292
#&gt; SRR1576839     1   0.924      0.252 0.420 0.136 0.152 0.292
#&gt; SRR1576840     1   0.924      0.252 0.420 0.136 0.152 0.292
#&gt; SRR1576841     3   0.533      0.586 0.000 0.036 0.680 0.284
#&gt; SRR1576842     3   0.533      0.586 0.000 0.036 0.680 0.284
#&gt; SRR1576843     3   0.533      0.586 0.000 0.036 0.680 0.284
#&gt; SRR1576844     4   0.535      0.561 0.000 0.180 0.084 0.736
#&gt; SRR1576845     4   0.535      0.561 0.000 0.180 0.084 0.736
#&gt; SRR1576846     4   0.535      0.561 0.000 0.180 0.084 0.736
#&gt; SRR1576847     4   0.535      0.561 0.000 0.180 0.084 0.736
#&gt; SRR1576848     4   0.535      0.561 0.000 0.180 0.084 0.736
#&gt; SRR1576849     4   0.535      0.561 0.000 0.180 0.084 0.736
#&gt; SRR1576850     4   0.695      0.571 0.128 0.176 0.036 0.660
#&gt; SRR1576851     4   0.695      0.571 0.128 0.176 0.036 0.660
#&gt; SRR1576852     4   0.695      0.571 0.128 0.176 0.036 0.660
#&gt; SRR1576853     4   0.882     -0.144 0.372 0.100 0.124 0.404
#&gt; SRR1576854     4   0.882     -0.144 0.372 0.100 0.124 0.404
#&gt; SRR1576855     4   0.882     -0.144 0.372 0.100 0.124 0.404
#&gt; SRR1576856     4   0.882     -0.144 0.372 0.100 0.124 0.404
#&gt; SRR1576857     3   0.000      0.846 0.000 0.000 1.000 0.000
#&gt; SRR1576858     3   0.000      0.846 0.000 0.000 1.000 0.000
#&gt; SRR1576859     3   0.000      0.846 0.000 0.000 1.000 0.000
#&gt; SRR1576860     3   0.000      0.846 0.000 0.000 1.000 0.000
#&gt; SRR1576861     3   0.000      0.846 0.000 0.000 1.000 0.000
#&gt; SRR1576862     3   0.000      0.846 0.000 0.000 1.000 0.000
#&gt; SRR1576863     4   0.751      0.373 0.128 0.104 0.124 0.644
#&gt; SRR1576864     4   0.751      0.373 0.128 0.104 0.124 0.644
#&gt; SRR1576865     4   0.751      0.373 0.128 0.104 0.124 0.644
#&gt; SRR1576866     4   0.751      0.373 0.128 0.104 0.124 0.644
#&gt; SRR1576867     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576874     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576875     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576876     2   0.000      0.982 0.000 1.000 0.000 0.000
#&gt; SRR1576877     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576878     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576879     1   0.000      0.778 1.000 0.000 0.000 0.000
#&gt; SRR1576880     4   0.506      0.563 0.004 0.160 0.068 0.768
#&gt; SRR1576881     4   0.506      0.563 0.004 0.160 0.068 0.768
#&gt; SRR1576882     4   0.506      0.563 0.004 0.160 0.068 0.768
#&gt; SRR1576883     2   0.121      0.963 0.000 0.960 0.000 0.040
#&gt; SRR1576884     2   0.121      0.963 0.000 0.960 0.000 0.040
#&gt; SRR1576885     2   0.121      0.963 0.000 0.960 0.000 0.040
#&gt; SRR1576886     2   0.121      0.963 0.000 0.960 0.000 0.040
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576837     5  0.1270      0.525 0.000 0.00 0.000 0.052 0.948
#&gt; SRR1576838     5  0.1270      0.525 0.000 0.00 0.000 0.052 0.948
#&gt; SRR1576839     5  0.1270      0.525 0.000 0.00 0.000 0.052 0.948
#&gt; SRR1576840     5  0.1270      0.525 0.000 0.00 0.000 0.052 0.948
#&gt; SRR1576841     3  0.4392      0.540 0.008 0.00 0.612 0.380 0.000
#&gt; SRR1576842     3  0.4392      0.540 0.008 0.00 0.612 0.380 0.000
#&gt; SRR1576843     3  0.4392      0.540 0.008 0.00 0.612 0.380 0.000
#&gt; SRR1576844     4  0.0000      0.824 0.000 0.00 0.000 1.000 0.000
#&gt; SRR1576845     4  0.0000      0.824 0.000 0.00 0.000 1.000 0.000
#&gt; SRR1576846     4  0.0000      0.824 0.000 0.00 0.000 1.000 0.000
#&gt; SRR1576847     4  0.0290      0.823 0.000 0.00 0.008 0.992 0.000
#&gt; SRR1576848     4  0.0290      0.823 0.000 0.00 0.008 0.992 0.000
#&gt; SRR1576849     4  0.0290      0.823 0.000 0.00 0.008 0.992 0.000
#&gt; SRR1576850     4  0.5983      0.356 0.380 0.00 0.000 0.504 0.116
#&gt; SRR1576851     4  0.5983      0.356 0.380 0.00 0.000 0.504 0.116
#&gt; SRR1576852     4  0.5983      0.356 0.380 0.00 0.000 0.504 0.116
#&gt; SRR1576853     5  0.3177      0.741 0.208 0.00 0.000 0.000 0.792
#&gt; SRR1576854     5  0.3177      0.741 0.208 0.00 0.000 0.000 0.792
#&gt; SRR1576855     5  0.3177      0.741 0.208 0.00 0.000 0.000 0.792
#&gt; SRR1576856     5  0.3177      0.741 0.208 0.00 0.000 0.000 0.792
#&gt; SRR1576857     3  0.0000      0.840 0.000 0.00 1.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      0.840 0.000 0.00 1.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.840 0.000 0.00 1.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      0.840 0.000 0.00 1.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      0.840 0.000 0.00 1.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      0.840 0.000 0.00 1.000 0.000 0.000
#&gt; SRR1576863     5  0.4278      0.675 0.452 0.00 0.000 0.000 0.548
#&gt; SRR1576864     5  0.4278      0.675 0.452 0.00 0.000 0.000 0.548
#&gt; SRR1576865     5  0.4278      0.675 0.452 0.00 0.000 0.000 0.548
#&gt; SRR1576866     5  0.4278      0.675 0.452 0.00 0.000 0.000 0.548
#&gt; SRR1576867     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576868     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576869     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576870     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576871     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576872     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576873     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1576877     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576878     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576879     1  0.4278      1.000 0.548 0.00 0.000 0.000 0.452
#&gt; SRR1576880     4  0.0932      0.817 0.004 0.00 0.020 0.972 0.004
#&gt; SRR1576881     4  0.0932      0.817 0.004 0.00 0.020 0.972 0.004
#&gt; SRR1576882     4  0.0932      0.817 0.004 0.00 0.020 0.972 0.004
#&gt; SRR1576883     2  0.1043      0.973 0.040 0.96 0.000 0.000 0.000
#&gt; SRR1576884     2  0.1043      0.973 0.040 0.96 0.000 0.000 0.000
#&gt; SRR1576885     2  0.1043      0.973 0.040 0.96 0.000 0.000 0.000
#&gt; SRR1576886     2  0.1043      0.973 0.040 0.96 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3    p4    p5    p6
#&gt; SRR1576833     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576837     5   0.234      0.804 0.148 0.00 0.000 0.000 0.852 0.000
#&gt; SRR1576838     5   0.234      0.804 0.148 0.00 0.000 0.000 0.852 0.000
#&gt; SRR1576839     5   0.234      0.804 0.148 0.00 0.000 0.000 0.852 0.000
#&gt; SRR1576840     5   0.234      0.804 0.148 0.00 0.000 0.000 0.852 0.000
#&gt; SRR1576841     3   0.447      0.546 0.000 0.00 0.612 0.352 0.032 0.004
#&gt; SRR1576842     3   0.447      0.546 0.000 0.00 0.612 0.352 0.032 0.004
#&gt; SRR1576843     3   0.447      0.546 0.000 0.00 0.612 0.352 0.032 0.004
#&gt; SRR1576844     4   0.000      0.809 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; SRR1576845     4   0.000      0.809 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; SRR1576846     4   0.000      0.809 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; SRR1576847     4   0.000      0.809 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; SRR1576848     4   0.000      0.809 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; SRR1576849     4   0.000      0.809 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; SRR1576850     4   0.558      0.208 0.000 0.00 0.000 0.480 0.144 0.376
#&gt; SRR1576851     4   0.558      0.208 0.000 0.00 0.000 0.480 0.144 0.376
#&gt; SRR1576852     4   0.558      0.208 0.000 0.00 0.000 0.480 0.144 0.376
#&gt; SRR1576853     5   0.509      0.801 0.144 0.00 0.000 0.000 0.624 0.232
#&gt; SRR1576854     5   0.509      0.801 0.144 0.00 0.000 0.000 0.624 0.232
#&gt; SRR1576855     5   0.509      0.801 0.144 0.00 0.000 0.000 0.624 0.232
#&gt; SRR1576856     5   0.509      0.801 0.144 0.00 0.000 0.000 0.624 0.232
#&gt; SRR1576857     3   0.000      0.826 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3   0.000      0.826 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3   0.000      0.826 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3   0.000      0.826 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3   0.000      0.826 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3   0.000      0.826 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; SRR1576863     6   0.000      1.000 0.000 0.00 0.000 0.000 0.000 1.000
#&gt; SRR1576864     6   0.000      1.000 0.000 0.00 0.000 0.000 0.000 1.000
#&gt; SRR1576865     6   0.000      1.000 0.000 0.00 0.000 0.000 0.000 1.000
#&gt; SRR1576866     6   0.000      1.000 0.000 0.00 0.000 0.000 0.000 1.000
#&gt; SRR1576867     1   0.000      0.864 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1   0.000      0.864 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1   0.000      0.864 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1   0.000      0.864 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1   0.000      0.864 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1   0.000      0.864 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2   0.000      0.948 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; SRR1576877     1   0.343      0.655 0.696 0.00 0.000 0.000 0.304 0.000
#&gt; SRR1576878     1   0.343      0.655 0.696 0.00 0.000 0.000 0.304 0.000
#&gt; SRR1576879     1   0.343      0.655 0.696 0.00 0.000 0.000 0.304 0.000
#&gt; SRR1576880     4   0.144      0.765 0.000 0.00 0.072 0.928 0.000 0.000
#&gt; SRR1576881     4   0.144      0.765 0.000 0.00 0.072 0.928 0.000 0.000
#&gt; SRR1576882     4   0.144      0.765 0.000 0.00 0.072 0.928 0.000 0.000
#&gt; SRR1576883     2   0.274      0.892 0.000 0.84 0.000 0.000 0.144 0.016
#&gt; SRR1576884     2   0.274      0.892 0.000 0.84 0.000 0.000 0.144 0.016
#&gt; SRR1576885     2   0.274      0.892 0.000 0.84 0.000 0.000 0.144 0.016
#&gt; SRR1576886     2   0.274      0.892 0.000 0.84 0.000 0.000 0.144 0.016
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.624           0.906       0.929         0.4932 0.491   0.491
#> 3 3 0.880           0.893       0.954         0.2736 0.822   0.651
#> 4 4 0.860           0.900       0.942         0.1974 0.856   0.611
#> 5 5 0.845           0.794       0.897         0.0721 0.855   0.499
#> 6 6 0.901           0.795       0.874         0.0259 0.971   0.849
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576834     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576835     2  0.2778      0.966 0.048 0.952
#&gt; SRR1576836     2  0.2778      0.966 0.048 0.952
#&gt; SRR1576837     1  0.1414      0.892 0.980 0.020
#&gt; SRR1576838     1  0.1414      0.892 0.980 0.020
#&gt; SRR1576839     1  0.5519      0.879 0.872 0.128
#&gt; SRR1576840     1  0.5519      0.879 0.872 0.128
#&gt; SRR1576841     1  0.6438      0.869 0.836 0.164
#&gt; SRR1576842     1  0.6438      0.869 0.836 0.164
#&gt; SRR1576843     1  0.6438      0.869 0.836 0.164
#&gt; SRR1576844     2  0.1184      0.948 0.016 0.984
#&gt; SRR1576845     2  0.1184      0.948 0.016 0.984
#&gt; SRR1576846     2  0.1184      0.948 0.016 0.984
#&gt; SRR1576847     2  0.1633      0.945 0.024 0.976
#&gt; SRR1576848     2  0.1633      0.945 0.024 0.976
#&gt; SRR1576849     2  0.1843      0.943 0.028 0.972
#&gt; SRR1576850     2  0.0376      0.956 0.004 0.996
#&gt; SRR1576851     2  0.0376      0.956 0.004 0.996
#&gt; SRR1576852     2  0.0376      0.956 0.004 0.996
#&gt; SRR1576853     1  0.4690      0.875 0.900 0.100
#&gt; SRR1576854     1  0.4298      0.882 0.912 0.088
#&gt; SRR1576855     1  0.9635      0.384 0.612 0.388
#&gt; SRR1576856     1  0.9323      0.486 0.652 0.348
#&gt; SRR1576857     1  0.6343      0.872 0.840 0.160
#&gt; SRR1576858     1  0.6343      0.872 0.840 0.160
#&gt; SRR1576859     1  0.6343      0.872 0.840 0.160
#&gt; SRR1576860     1  0.5519      0.881 0.872 0.128
#&gt; SRR1576861     1  0.5519      0.881 0.872 0.128
#&gt; SRR1576862     1  0.5519      0.881 0.872 0.128
#&gt; SRR1576863     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576864     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576865     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576866     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576867     1  0.1184      0.892 0.984 0.016
#&gt; SRR1576868     1  0.1184      0.892 0.984 0.016
#&gt; SRR1576869     1  0.1184      0.892 0.984 0.016
#&gt; SRR1576870     1  0.1184      0.892 0.984 0.016
#&gt; SRR1576871     1  0.1184      0.892 0.984 0.016
#&gt; SRR1576872     1  0.1184      0.892 0.984 0.016
#&gt; SRR1576873     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576874     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576875     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576876     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576877     1  0.3584      0.884 0.932 0.068
#&gt; SRR1576878     1  0.3431      0.885 0.936 0.064
#&gt; SRR1576879     1  0.3431      0.885 0.936 0.064
#&gt; SRR1576880     2  0.0376      0.955 0.004 0.996
#&gt; SRR1576881     2  0.0376      0.955 0.004 0.996
#&gt; SRR1576882     2  0.0376      0.955 0.004 0.996
#&gt; SRR1576883     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576884     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576885     2  0.2948      0.966 0.052 0.948
#&gt; SRR1576886     2  0.2948      0.966 0.052 0.948
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576837     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576838     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576839     1  0.7334      0.505 0.624 0.328 0.048
#&gt; SRR1576840     1  0.8126      0.421 0.564 0.356 0.080
#&gt; SRR1576841     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576842     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576843     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576844     2  0.0892      0.982 0.000 0.980 0.020
#&gt; SRR1576845     2  0.0892      0.982 0.000 0.980 0.020
#&gt; SRR1576846     2  0.0892      0.982 0.000 0.980 0.020
#&gt; SRR1576847     3  0.6095      0.449 0.000 0.392 0.608
#&gt; SRR1576848     3  0.6095      0.449 0.000 0.392 0.608
#&gt; SRR1576849     3  0.6026      0.480 0.000 0.376 0.624
#&gt; SRR1576850     2  0.1411      0.968 0.000 0.964 0.036
#&gt; SRR1576851     2  0.1411      0.968 0.000 0.964 0.036
#&gt; SRR1576852     2  0.1031      0.979 0.000 0.976 0.024
#&gt; SRR1576853     1  0.4062      0.772 0.836 0.164 0.000
#&gt; SRR1576854     1  0.3752      0.793 0.856 0.144 0.000
#&gt; SRR1576855     1  0.0424      0.909 0.992 0.008 0.000
#&gt; SRR1576856     1  0.0424      0.909 0.992 0.008 0.000
#&gt; SRR1576857     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576858     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576859     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576860     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576861     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576862     3  0.0000      0.867 0.000 0.000 1.000
#&gt; SRR1576863     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576864     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576865     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576866     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576867     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576877     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.913 1.000 0.000 0.000
#&gt; SRR1576880     2  0.0592      0.987 0.000 0.988 0.012
#&gt; SRR1576881     2  0.0747      0.984 0.000 0.984 0.016
#&gt; SRR1576882     2  0.0424      0.988 0.000 0.992 0.008
#&gt; SRR1576883     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576884     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576885     2  0.0000      0.991 0.000 1.000 0.000
#&gt; SRR1576886     2  0.0000      0.991 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576837     1  0.1811      0.922 0.948 0.020 0.004 0.028
#&gt; SRR1576838     1  0.1811      0.922 0.948 0.020 0.004 0.028
#&gt; SRR1576839     1  0.5694      0.708 0.724 0.040 0.028 0.208
#&gt; SRR1576840     1  0.6221      0.672 0.692 0.040 0.048 0.220
#&gt; SRR1576841     3  0.0188      0.949 0.000 0.000 0.996 0.004
#&gt; SRR1576842     3  0.0188      0.949 0.000 0.000 0.996 0.004
#&gt; SRR1576843     3  0.0188      0.949 0.000 0.000 0.996 0.004
#&gt; SRR1576844     2  0.3877      0.827 0.000 0.840 0.112 0.048
#&gt; SRR1576845     2  0.3877      0.827 0.000 0.840 0.112 0.048
#&gt; SRR1576846     2  0.3877      0.827 0.000 0.840 0.112 0.048
#&gt; SRR1576847     3  0.4046      0.827 0.000 0.124 0.828 0.048
#&gt; SRR1576848     3  0.4046      0.827 0.000 0.124 0.828 0.048
#&gt; SRR1576849     3  0.3934      0.835 0.000 0.116 0.836 0.048
#&gt; SRR1576850     4  0.1004      0.948 0.000 0.024 0.004 0.972
#&gt; SRR1576851     4  0.1004      0.948 0.000 0.024 0.004 0.972
#&gt; SRR1576852     4  0.1004      0.948 0.000 0.024 0.004 0.972
#&gt; SRR1576853     4  0.1733      0.958 0.028 0.024 0.000 0.948
#&gt; SRR1576854     4  0.1733      0.958 0.028 0.024 0.000 0.948
#&gt; SRR1576855     4  0.1706      0.956 0.036 0.016 0.000 0.948
#&gt; SRR1576856     4  0.1706      0.956 0.036 0.016 0.000 0.948
#&gt; SRR1576857     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; SRR1576858     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; SRR1576859     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; SRR1576860     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; SRR1576861     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; SRR1576862     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; SRR1576863     4  0.0921      0.968 0.000 0.028 0.000 0.972
#&gt; SRR1576864     4  0.0921      0.968 0.000 0.028 0.000 0.972
#&gt; SRR1576865     4  0.0921      0.968 0.000 0.028 0.000 0.972
#&gt; SRR1576866     4  0.0921      0.968 0.000 0.028 0.000 0.972
#&gt; SRR1576867     1  0.0000      0.946 1.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.946 1.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.946 1.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      0.946 1.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.946 1.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.946 1.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576877     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; SRR1576878     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; SRR1576879     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; SRR1576880     2  0.5732      0.636 0.000 0.672 0.264 0.064
#&gt; SRR1576881     2  0.5759      0.629 0.000 0.668 0.268 0.064
#&gt; SRR1576882     2  0.5925      0.596 0.000 0.648 0.284 0.068
#&gt; SRR1576883     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      0.910 0.000 1.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      0.910 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576837     1  0.6478     0.4103 0.520 0.004 0.264 0.000 0.212
#&gt; SRR1576838     1  0.6414     0.4266 0.532 0.004 0.260 0.000 0.204
#&gt; SRR1576839     3  0.5219     0.4077 0.220 0.044 0.700 0.000 0.036
#&gt; SRR1576840     3  0.5122     0.4260 0.208 0.048 0.712 0.000 0.032
#&gt; SRR1576841     3  0.4302     0.0878 0.000 0.000 0.520 0.480 0.000
#&gt; SRR1576842     3  0.4305     0.0630 0.000 0.000 0.512 0.488 0.000
#&gt; SRR1576843     3  0.4305     0.0630 0.000 0.000 0.512 0.488 0.000
#&gt; SRR1576844     4  0.1310     0.8145 0.000 0.024 0.020 0.956 0.000
#&gt; SRR1576845     4  0.1493     0.8134 0.000 0.028 0.024 0.948 0.000
#&gt; SRR1576846     4  0.1485     0.8115 0.000 0.032 0.020 0.948 0.000
#&gt; SRR1576847     4  0.3305     0.6821 0.000 0.000 0.224 0.776 0.000
#&gt; SRR1576848     4  0.3336     0.6766 0.000 0.000 0.228 0.772 0.000
#&gt; SRR1576849     4  0.3305     0.6821 0.000 0.000 0.224 0.776 0.000
#&gt; SRR1576850     4  0.3534     0.7080 0.000 0.000 0.000 0.744 0.256
#&gt; SRR1576851     4  0.3534     0.7080 0.000 0.000 0.000 0.744 0.256
#&gt; SRR1576852     4  0.3534     0.7080 0.000 0.000 0.000 0.744 0.256
#&gt; SRR1576853     5  0.0000     0.9977 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576854     5  0.0000     0.9977 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576855     5  0.0000     0.9977 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576856     5  0.0000     0.9977 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576857     3  0.1270     0.7464 0.000 0.000 0.948 0.052 0.000
#&gt; SRR1576858     3  0.1270     0.7464 0.000 0.000 0.948 0.052 0.000
#&gt; SRR1576859     3  0.1270     0.7464 0.000 0.000 0.948 0.052 0.000
#&gt; SRR1576860     3  0.1270     0.7464 0.000 0.000 0.948 0.052 0.000
#&gt; SRR1576861     3  0.1270     0.7464 0.000 0.000 0.948 0.052 0.000
#&gt; SRR1576862     3  0.1270     0.7464 0.000 0.000 0.948 0.052 0.000
#&gt; SRR1576863     5  0.0162     0.9977 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1576864     5  0.0162     0.9977 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1576865     5  0.0162     0.9977 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1576866     5  0.0162     0.9977 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1576867     1  0.0290     0.8534 0.992 0.000 0.000 0.008 0.000
#&gt; SRR1576868     1  0.0290     0.8534 0.992 0.000 0.000 0.008 0.000
#&gt; SRR1576869     1  0.0290     0.8534 0.992 0.000 0.000 0.008 0.000
#&gt; SRR1576870     1  0.0162     0.8519 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1576871     1  0.0162     0.8519 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1576872     1  0.0162     0.8519 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1576873     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.2690     0.7942 0.844 0.000 0.000 0.156 0.000
#&gt; SRR1576878     1  0.2690     0.7942 0.844 0.000 0.000 0.156 0.000
#&gt; SRR1576879     1  0.2690     0.7942 0.844 0.000 0.000 0.156 0.000
#&gt; SRR1576880     4  0.0324     0.8147 0.004 0.004 0.000 0.992 0.000
#&gt; SRR1576881     4  0.0324     0.8147 0.004 0.004 0.000 0.992 0.000
#&gt; SRR1576882     4  0.0324     0.8147 0.004 0.004 0.000 0.992 0.000
#&gt; SRR1576883     2  0.0000     0.9982 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576884     2  0.0162     0.9958 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576885     2  0.0290     0.9933 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1576886     2  0.0290     0.9933 0.000 0.992 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     1  0.6172      0.467 0.584 0.008 0.092 0.000 0.244 0.072
#&gt; SRR1576838     1  0.6130      0.471 0.588 0.008 0.088 0.000 0.244 0.072
#&gt; SRR1576839     5  0.8313     -0.235 0.276 0.104 0.268 0.000 0.280 0.072
#&gt; SRR1576840     3  0.8278     -0.127 0.252 0.100 0.304 0.000 0.272 0.072
#&gt; SRR1576841     3  0.4319      0.413 0.000 0.000 0.576 0.400 0.000 0.024
#&gt; SRR1576842     3  0.4301      0.427 0.000 0.000 0.584 0.392 0.000 0.024
#&gt; SRR1576843     3  0.4348      0.379 0.000 0.000 0.560 0.416 0.000 0.024
#&gt; SRR1576844     4  0.0820      0.924 0.000 0.000 0.012 0.972 0.000 0.016
#&gt; SRR1576845     4  0.0909      0.924 0.000 0.000 0.012 0.968 0.000 0.020
#&gt; SRR1576846     4  0.0820      0.924 0.000 0.000 0.012 0.972 0.000 0.016
#&gt; SRR1576847     4  0.2122      0.917 0.000 0.000 0.024 0.900 0.000 0.076
#&gt; SRR1576848     4  0.2122      0.917 0.000 0.000 0.024 0.900 0.000 0.076
#&gt; SRR1576849     4  0.2122      0.917 0.000 0.000 0.024 0.900 0.000 0.076
#&gt; SRR1576850     4  0.2420      0.909 0.000 0.000 0.000 0.884 0.040 0.076
#&gt; SRR1576851     4  0.2488      0.907 0.000 0.000 0.000 0.880 0.044 0.076
#&gt; SRR1576852     4  0.2488      0.907 0.000 0.000 0.000 0.880 0.044 0.076
#&gt; SRR1576853     5  0.0146      0.786 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1576854     5  0.0146      0.786 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1576855     5  0.0000      0.787 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576856     5  0.0000      0.787 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576857     3  0.0000      0.737 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      0.737 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.737 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      0.737 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      0.737 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      0.737 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     5  0.2883      0.774 0.000 0.000 0.000 0.000 0.788 0.212
#&gt; SRR1576864     5  0.2883      0.774 0.000 0.000 0.000 0.000 0.788 0.212
#&gt; SRR1576865     5  0.2883      0.774 0.000 0.000 0.000 0.000 0.788 0.212
#&gt; SRR1576866     5  0.2883      0.774 0.000 0.000 0.000 0.000 0.788 0.212
#&gt; SRR1576867     1  0.0547      0.766 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; SRR1576868     1  0.0547      0.766 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; SRR1576869     1  0.0547      0.766 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; SRR1576870     1  0.0000      0.775 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      0.775 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      0.775 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576877     6  0.4420      0.993 0.360 0.000 0.000 0.036 0.000 0.604
#&gt; SRR1576878     6  0.4420      0.994 0.360 0.000 0.000 0.036 0.000 0.604
#&gt; SRR1576879     6  0.4470      0.993 0.356 0.000 0.000 0.040 0.000 0.604
#&gt; SRR1576880     4  0.1196      0.918 0.000 0.000 0.008 0.952 0.000 0.040
#&gt; SRR1576881     4  0.1124      0.920 0.000 0.000 0.008 0.956 0.000 0.036
#&gt; SRR1576882     4  0.0972      0.922 0.000 0.000 0.008 0.964 0.000 0.028
#&gt; SRR1576883     2  0.0405      0.988 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; SRR1576884     2  0.0405      0.988 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; SRR1576885     2  0.0972      0.972 0.000 0.964 0.000 0.008 0.000 0.028
#&gt; SRR1576886     2  0.0806      0.978 0.000 0.972 0.000 0.008 0.000 0.020
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.5092 0.491   0.491
#> 3 3 0.893           0.951       0.956         0.2111 0.885   0.765
#> 4 4 1.000           0.991       0.990         0.0756 0.962   0.900
#> 5 5 1.000           0.993       0.993         0.0991 0.933   0.802
#> 6 6 0.873           0.958       0.860         0.0817 0.899   0.629
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 4
```

There is also optional best $k$ = 2 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1576833     2       0          1  0  1
#&gt; SRR1576834     2       0          1  0  1
#&gt; SRR1576835     2       0          1  0  1
#&gt; SRR1576836     2       0          1  0  1
#&gt; SRR1576837     1       0          1  1  0
#&gt; SRR1576838     1       0          1  1  0
#&gt; SRR1576839     1       0          1  1  0
#&gt; SRR1576840     1       0          1  1  0
#&gt; SRR1576841     1       0          1  1  0
#&gt; SRR1576842     1       0          1  1  0
#&gt; SRR1576843     1       0          1  1  0
#&gt; SRR1576844     2       0          1  0  1
#&gt; SRR1576845     2       0          1  0  1
#&gt; SRR1576846     2       0          1  0  1
#&gt; SRR1576847     2       0          1  0  1
#&gt; SRR1576848     2       0          1  0  1
#&gt; SRR1576849     2       0          1  0  1
#&gt; SRR1576850     2       0          1  0  1
#&gt; SRR1576851     2       0          1  0  1
#&gt; SRR1576852     2       0          1  0  1
#&gt; SRR1576853     1       0          1  1  0
#&gt; SRR1576854     1       0          1  1  0
#&gt; SRR1576855     1       0          1  1  0
#&gt; SRR1576856     1       0          1  1  0
#&gt; SRR1576857     1       0          1  1  0
#&gt; SRR1576858     1       0          1  1  0
#&gt; SRR1576859     1       0          1  1  0
#&gt; SRR1576860     1       0          1  1  0
#&gt; SRR1576861     1       0          1  1  0
#&gt; SRR1576862     1       0          1  1  0
#&gt; SRR1576863     2       0          1  0  1
#&gt; SRR1576864     2       0          1  0  1
#&gt; SRR1576865     2       0          1  0  1
#&gt; SRR1576866     2       0          1  0  1
#&gt; SRR1576867     1       0          1  1  0
#&gt; SRR1576868     1       0          1  1  0
#&gt; SRR1576869     1       0          1  1  0
#&gt; SRR1576870     1       0          1  1  0
#&gt; SRR1576871     1       0          1  1  0
#&gt; SRR1576872     1       0          1  1  0
#&gt; SRR1576873     2       0          1  0  1
#&gt; SRR1576874     2       0          1  0  1
#&gt; SRR1576875     2       0          1  0  1
#&gt; SRR1576876     2       0          1  0  1
#&gt; SRR1576877     1       0          1  1  0
#&gt; SRR1576878     1       0          1  1  0
#&gt; SRR1576879     1       0          1  1  0
#&gt; SRR1576880     2       0          1  0  1
#&gt; SRR1576881     2       0          1  0  1
#&gt; SRR1576882     2       0          1  0  1
#&gt; SRR1576883     2       0          1  0  1
#&gt; SRR1576884     2       0          1  0  1
#&gt; SRR1576885     2       0          1  0  1
#&gt; SRR1576886     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1576833     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576834     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576835     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576836     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576837     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576838     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576839     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576840     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576841     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576842     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576843     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576844     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576845     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576846     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576847     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576848     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576849     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576850     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576851     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576852     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576853     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576854     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576855     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576856     1   0.000      1.000 1.000  0 0.000
#&gt; SRR1576857     3   0.000      0.808 0.000  0 1.000
#&gt; SRR1576858     3   0.000      0.808 0.000  0 1.000
#&gt; SRR1576859     3   0.000      0.808 0.000  0 1.000
#&gt; SRR1576860     3   0.000      0.808 0.000  0 1.000
#&gt; SRR1576861     3   0.000      0.808 0.000  0 1.000
#&gt; SRR1576862     3   0.000      0.808 0.000  0 1.000
#&gt; SRR1576863     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576864     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576865     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576866     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576867     3   0.510      0.845 0.248  0 0.752
#&gt; SRR1576868     3   0.510      0.845 0.248  0 0.752
#&gt; SRR1576869     3   0.510      0.845 0.248  0 0.752
#&gt; SRR1576870     3   0.510      0.845 0.248  0 0.752
#&gt; SRR1576871     3   0.510      0.845 0.248  0 0.752
#&gt; SRR1576872     3   0.510      0.845 0.248  0 0.752
#&gt; SRR1576873     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576874     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576875     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576876     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576877     3   0.546      0.812 0.288  0 0.712
#&gt; SRR1576878     3   0.546      0.812 0.288  0 0.712
#&gt; SRR1576879     3   0.546      0.812 0.288  0 0.712
#&gt; SRR1576880     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576881     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576882     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576883     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576884     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576885     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1576886     2   0.000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; SRR1576833     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576834     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576835     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576836     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576837     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576838     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576839     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576840     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576841     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576842     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576843     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576844     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576845     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576846     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576847     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576848     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576849     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576850     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576851     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576852     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576853     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576854     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576855     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576856     4   0.000      1.000 0.00 0.00 0.00 1.00
#&gt; SRR1576857     3   0.000      1.000 0.00 0.00 1.00 0.00
#&gt; SRR1576858     3   0.000      1.000 0.00 0.00 1.00 0.00
#&gt; SRR1576859     3   0.000      1.000 0.00 0.00 1.00 0.00
#&gt; SRR1576860     3   0.000      1.000 0.00 0.00 1.00 0.00
#&gt; SRR1576861     3   0.000      1.000 0.00 0.00 1.00 0.00
#&gt; SRR1576862     3   0.000      1.000 0.00 0.00 1.00 0.00
#&gt; SRR1576863     2   0.121      0.967 0.04 0.96 0.00 0.00
#&gt; SRR1576864     2   0.121      0.967 0.04 0.96 0.00 0.00
#&gt; SRR1576865     2   0.121      0.967 0.04 0.96 0.00 0.00
#&gt; SRR1576866     2   0.121      0.967 0.04 0.96 0.00 0.00
#&gt; SRR1576867     1   0.121      0.979 0.96 0.00 0.04 0.00
#&gt; SRR1576868     1   0.121      0.979 0.96 0.00 0.04 0.00
#&gt; SRR1576869     1   0.121      0.979 0.96 0.00 0.04 0.00
#&gt; SRR1576870     1   0.121      0.979 0.96 0.00 0.04 0.00
#&gt; SRR1576871     1   0.121      0.979 0.96 0.00 0.04 0.00
#&gt; SRR1576872     1   0.121      0.979 0.96 0.00 0.04 0.00
#&gt; SRR1576873     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576874     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576875     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576876     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576877     1   0.121      0.959 0.96 0.00 0.00 0.04
#&gt; SRR1576878     1   0.121      0.959 0.96 0.00 0.00 0.04
#&gt; SRR1576879     1   0.121      0.959 0.96 0.00 0.00 0.04
#&gt; SRR1576880     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576881     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576882     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576883     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576884     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576885     2   0.000      0.995 0.00 1.00 0.00 0.00
#&gt; SRR1576886     2   0.000      0.995 0.00 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1 p2   p3 p4   p5
#&gt; SRR1576833     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576834     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576835     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576836     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576837     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576838     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576839     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576840     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576841     5   0.104      0.968 0.04  0 0.00  0 0.96
#&gt; SRR1576842     5   0.104      0.968 0.04  0 0.00  0 0.96
#&gt; SRR1576843     5   0.104      0.968 0.04  0 0.00  0 0.96
#&gt; SRR1576844     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576845     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576846     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576847     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576848     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576849     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576850     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576851     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576852     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576853     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576854     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576855     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576856     5   0.000      0.988 0.00  0 0.00  0 1.00
#&gt; SRR1576857     3   0.000      1.000 0.00  0 1.00  0 0.00
#&gt; SRR1576858     3   0.000      1.000 0.00  0 1.00  0 0.00
#&gt; SRR1576859     3   0.000      1.000 0.00  0 1.00  0 0.00
#&gt; SRR1576860     3   0.000      1.000 0.00  0 1.00  0 0.00
#&gt; SRR1576861     3   0.000      1.000 0.00  0 1.00  0 0.00
#&gt; SRR1576862     3   0.000      1.000 0.00  0 1.00  0 0.00
#&gt; SRR1576863     2   0.000      1.000 0.00  1 0.00  0 0.00
#&gt; SRR1576864     2   0.000      1.000 0.00  1 0.00  0 0.00
#&gt; SRR1576865     2   0.000      1.000 0.00  1 0.00  0 0.00
#&gt; SRR1576866     2   0.000      1.000 0.00  1 0.00  0 0.00
#&gt; SRR1576867     1   0.104      0.982 0.96  0 0.04  0 0.00
#&gt; SRR1576868     1   0.104      0.982 0.96  0 0.04  0 0.00
#&gt; SRR1576869     1   0.104      0.982 0.96  0 0.04  0 0.00
#&gt; SRR1576870     1   0.104      0.982 0.96  0 0.04  0 0.00
#&gt; SRR1576871     1   0.104      0.982 0.96  0 0.04  0 0.00
#&gt; SRR1576872     1   0.104      0.982 0.96  0 0.04  0 0.00
#&gt; SRR1576873     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576874     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576875     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576876     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576877     1   0.000      0.964 1.00  0 0.00  0 0.00
#&gt; SRR1576878     1   0.000      0.964 1.00  0 0.00  0 0.00
#&gt; SRR1576879     1   0.000      0.964 1.00  0 0.00  0 0.00
#&gt; SRR1576880     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576881     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576882     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576883     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576884     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576885     4   0.000      1.000 0.00  0 0.00  1 0.00
#&gt; SRR1576886     4   0.000      1.000 0.00  0 0.00  1 0.00
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1    p2 p3    p4    p5 p6
#&gt; SRR1576833     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576834     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576835     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576836     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576837     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576838     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576839     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576840     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576841     5   0.343      0.755 0.00 0.304  0 0.000 0.696  0
#&gt; SRR1576842     5   0.343      0.755 0.00 0.304  0 0.000 0.696  0
#&gt; SRR1576843     5   0.343      0.755 0.00 0.304  0 0.000 0.696  0
#&gt; SRR1576844     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576845     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576846     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576847     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576848     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576849     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576850     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576851     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576852     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576853     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576854     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576855     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576856     5   0.000      0.917 0.00 0.000  0 0.000 1.000  0
#&gt; SRR1576857     3   0.000      1.000 0.00 0.000  1 0.000 0.000  0
#&gt; SRR1576858     3   0.000      1.000 0.00 0.000  1 0.000 0.000  0
#&gt; SRR1576859     3   0.000      1.000 0.00 0.000  1 0.000 0.000  0
#&gt; SRR1576860     3   0.000      1.000 0.00 0.000  1 0.000 0.000  0
#&gt; SRR1576861     3   0.000      1.000 0.00 0.000  1 0.000 0.000  0
#&gt; SRR1576862     3   0.000      1.000 0.00 0.000  1 0.000 0.000  0
#&gt; SRR1576863     6   0.000      1.000 0.00 0.000  0 0.000 0.000  1
#&gt; SRR1576864     6   0.000      1.000 0.00 0.000  0 0.000 0.000  1
#&gt; SRR1576865     6   0.000      1.000 0.00 0.000  0 0.000 0.000  1
#&gt; SRR1576866     6   0.000      1.000 0.00 0.000  0 0.000 0.000  1
#&gt; SRR1576867     1   0.000      0.928 1.00 0.000  0 0.000 0.000  0
#&gt; SRR1576868     1   0.000      0.928 1.00 0.000  0 0.000 0.000  0
#&gt; SRR1576869     1   0.000      0.928 1.00 0.000  0 0.000 0.000  0
#&gt; SRR1576870     1   0.000      0.928 1.00 0.000  0 0.000 0.000  0
#&gt; SRR1576871     1   0.000      0.928 1.00 0.000  0 0.000 0.000  0
#&gt; SRR1576872     1   0.000      0.928 1.00 0.000  0 0.000 0.000  0
#&gt; SRR1576873     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576874     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576875     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576876     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576877     1   0.294      0.848 0.78 0.220  0 0.000 0.000  0
#&gt; SRR1576878     1   0.294      0.848 0.78 0.220  0 0.000 0.000  0
#&gt; SRR1576879     1   0.294      0.848 0.78 0.220  0 0.000 0.000  0
#&gt; SRR1576880     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576881     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576882     4   0.000      1.000 0.00 0.000  0 1.000 0.000  0
#&gt; SRR1576883     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576884     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576885     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
#&gt; SRR1576886     2   0.387      1.000 0.00 0.516  0 0.484 0.000  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.792           0.844       0.933         0.4787 0.547   0.547
#> 3 3 0.452           0.766       0.794         0.2914 0.795   0.637
#> 4 4 0.584           0.680       0.683         0.1393 0.881   0.682
#> 5 5 0.631           0.352       0.682         0.0863 0.860   0.569
#> 6 6 0.685           0.797       0.766         0.0458 0.864   0.516
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.000      0.888 0.000 1.000
#&gt; SRR1576834     2   0.000      0.888 0.000 1.000
#&gt; SRR1576835     2   0.000      0.888 0.000 1.000
#&gt; SRR1576836     2   0.000      0.888 0.000 1.000
#&gt; SRR1576837     2   0.994      0.329 0.456 0.544
#&gt; SRR1576838     2   0.994      0.329 0.456 0.544
#&gt; SRR1576839     2   0.993      0.339 0.452 0.548
#&gt; SRR1576840     2   0.993      0.339 0.452 0.548
#&gt; SRR1576841     1   0.000      1.000 1.000 0.000
#&gt; SRR1576842     1   0.000      1.000 1.000 0.000
#&gt; SRR1576843     1   0.000      1.000 1.000 0.000
#&gt; SRR1576844     2   0.000      0.888 0.000 1.000
#&gt; SRR1576845     2   0.000      0.888 0.000 1.000
#&gt; SRR1576846     2   0.000      0.888 0.000 1.000
#&gt; SRR1576847     2   0.000      0.888 0.000 1.000
#&gt; SRR1576848     2   0.000      0.888 0.000 1.000
#&gt; SRR1576849     2   0.000      0.888 0.000 1.000
#&gt; SRR1576850     2   0.000      0.888 0.000 1.000
#&gt; SRR1576851     2   0.000      0.888 0.000 1.000
#&gt; SRR1576852     2   0.000      0.888 0.000 1.000
#&gt; SRR1576853     2   0.993      0.339 0.452 0.548
#&gt; SRR1576854     2   0.993      0.339 0.452 0.548
#&gt; SRR1576855     2   0.993      0.339 0.452 0.548
#&gt; SRR1576856     2   0.993      0.339 0.452 0.548
#&gt; SRR1576857     1   0.000      1.000 1.000 0.000
#&gt; SRR1576858     1   0.000      1.000 1.000 0.000
#&gt; SRR1576859     1   0.000      1.000 1.000 0.000
#&gt; SRR1576860     1   0.000      1.000 1.000 0.000
#&gt; SRR1576861     1   0.000      1.000 1.000 0.000
#&gt; SRR1576862     1   0.000      1.000 1.000 0.000
#&gt; SRR1576863     2   0.000      0.888 0.000 1.000
#&gt; SRR1576864     2   0.000      0.888 0.000 1.000
#&gt; SRR1576865     2   0.000      0.888 0.000 1.000
#&gt; SRR1576866     2   0.000      0.888 0.000 1.000
#&gt; SRR1576867     1   0.000      1.000 1.000 0.000
#&gt; SRR1576868     1   0.000      1.000 1.000 0.000
#&gt; SRR1576869     1   0.000      1.000 1.000 0.000
#&gt; SRR1576870     1   0.000      1.000 1.000 0.000
#&gt; SRR1576871     1   0.000      1.000 1.000 0.000
#&gt; SRR1576872     1   0.000      1.000 1.000 0.000
#&gt; SRR1576873     2   0.000      0.888 0.000 1.000
#&gt; SRR1576874     2   0.000      0.888 0.000 1.000
#&gt; SRR1576875     2   0.000      0.888 0.000 1.000
#&gt; SRR1576876     2   0.000      0.888 0.000 1.000
#&gt; SRR1576877     1   0.000      1.000 1.000 0.000
#&gt; SRR1576878     1   0.000      1.000 1.000 0.000
#&gt; SRR1576879     1   0.000      1.000 1.000 0.000
#&gt; SRR1576880     2   0.000      0.888 0.000 1.000
#&gt; SRR1576881     2   0.000      0.888 0.000 1.000
#&gt; SRR1576882     2   0.000      0.888 0.000 1.000
#&gt; SRR1576883     2   0.000      0.888 0.000 1.000
#&gt; SRR1576884     2   0.000      0.888 0.000 1.000
#&gt; SRR1576885     2   0.000      0.888 0.000 1.000
#&gt; SRR1576886     2   0.000      0.888 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0237      0.830 0.004 0.996 0.000
#&gt; SRR1576834     2  0.0237      0.830 0.004 0.996 0.000
#&gt; SRR1576835     2  0.0237      0.830 0.004 0.996 0.000
#&gt; SRR1576836     2  0.0237      0.830 0.004 0.996 0.000
#&gt; SRR1576837     1  0.8199      0.843 0.640 0.160 0.200
#&gt; SRR1576838     1  0.8199      0.843 0.640 0.160 0.200
#&gt; SRR1576839     1  0.8216      0.842 0.640 0.172 0.188
#&gt; SRR1576840     1  0.8216      0.842 0.640 0.172 0.188
#&gt; SRR1576841     1  0.7056      0.430 0.572 0.024 0.404
#&gt; SRR1576842     1  0.7056      0.430 0.572 0.024 0.404
#&gt; SRR1576843     1  0.7056      0.430 0.572 0.024 0.404
#&gt; SRR1576844     2  0.3816      0.823 0.148 0.852 0.000
#&gt; SRR1576845     2  0.3816      0.823 0.148 0.852 0.000
#&gt; SRR1576846     2  0.3816      0.823 0.148 0.852 0.000
#&gt; SRR1576847     2  0.5733      0.755 0.324 0.676 0.000
#&gt; SRR1576848     2  0.5733      0.755 0.324 0.676 0.000
#&gt; SRR1576849     2  0.5733      0.755 0.324 0.676 0.000
#&gt; SRR1576850     2  0.5678      0.758 0.316 0.684 0.000
#&gt; SRR1576851     2  0.5678      0.758 0.316 0.684 0.000
#&gt; SRR1576852     2  0.5678      0.758 0.316 0.684 0.000
#&gt; SRR1576853     1  0.8067      0.846 0.652 0.160 0.188
#&gt; SRR1576854     1  0.8067      0.846 0.652 0.160 0.188
#&gt; SRR1576855     1  0.8067      0.846 0.652 0.160 0.188
#&gt; SRR1576856     1  0.8067      0.846 0.652 0.160 0.188
#&gt; SRR1576857     3  0.3412      0.807 0.124 0.000 0.876
#&gt; SRR1576858     3  0.3412      0.807 0.124 0.000 0.876
#&gt; SRR1576859     3  0.3412      0.807 0.124 0.000 0.876
#&gt; SRR1576860     3  0.3412      0.807 0.124 0.000 0.876
#&gt; SRR1576861     3  0.3412      0.807 0.124 0.000 0.876
#&gt; SRR1576862     3  0.3412      0.807 0.124 0.000 0.876
#&gt; SRR1576863     2  0.3879      0.764 0.152 0.848 0.000
#&gt; SRR1576864     2  0.3879      0.764 0.152 0.848 0.000
#&gt; SRR1576865     2  0.3879      0.764 0.152 0.848 0.000
#&gt; SRR1576866     2  0.3879      0.764 0.152 0.848 0.000
#&gt; SRR1576867     3  0.2356      0.803 0.072 0.000 0.928
#&gt; SRR1576868     3  0.2356      0.803 0.072 0.000 0.928
#&gt; SRR1576869     3  0.2356      0.803 0.072 0.000 0.928
#&gt; SRR1576870     3  0.1163      0.815 0.028 0.000 0.972
#&gt; SRR1576871     3  0.1163      0.815 0.028 0.000 0.972
#&gt; SRR1576872     3  0.1163      0.815 0.028 0.000 0.972
#&gt; SRR1576873     2  0.2796      0.831 0.092 0.908 0.000
#&gt; SRR1576874     2  0.2796      0.831 0.092 0.908 0.000
#&gt; SRR1576875     2  0.2796      0.831 0.092 0.908 0.000
#&gt; SRR1576876     2  0.2796      0.831 0.092 0.908 0.000
#&gt; SRR1576877     3  0.5560      0.449 0.300 0.000 0.700
#&gt; SRR1576878     3  0.5560      0.449 0.300 0.000 0.700
#&gt; SRR1576879     3  0.5560      0.449 0.300 0.000 0.700
#&gt; SRR1576880     2  0.5678      0.757 0.316 0.684 0.000
#&gt; SRR1576881     2  0.5678      0.757 0.316 0.684 0.000
#&gt; SRR1576882     2  0.5678      0.757 0.316 0.684 0.000
#&gt; SRR1576883     2  0.1411      0.820 0.036 0.964 0.000
#&gt; SRR1576884     2  0.1411      0.820 0.036 0.964 0.000
#&gt; SRR1576885     2  0.1411      0.820 0.036 0.964 0.000
#&gt; SRR1576886     2  0.1411      0.820 0.036 0.964 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0672      0.673 0.000 0.984 0.008 0.008
#&gt; SRR1576834     2  0.0672      0.673 0.000 0.984 0.008 0.008
#&gt; SRR1576835     2  0.0336      0.676 0.000 0.992 0.008 0.000
#&gt; SRR1576836     2  0.0336      0.676 0.000 0.992 0.008 0.000
#&gt; SRR1576837     3  0.1930      0.918 0.004 0.056 0.936 0.004
#&gt; SRR1576838     3  0.1930      0.918 0.004 0.056 0.936 0.004
#&gt; SRR1576839     3  0.1824      0.916 0.000 0.060 0.936 0.004
#&gt; SRR1576840     3  0.1824      0.916 0.000 0.060 0.936 0.004
#&gt; SRR1576841     3  0.3970      0.775 0.124 0.004 0.836 0.036
#&gt; SRR1576842     3  0.3970      0.775 0.124 0.004 0.836 0.036
#&gt; SRR1576843     3  0.3970      0.775 0.124 0.004 0.836 0.036
#&gt; SRR1576844     2  0.6440     -0.639 0.008 0.536 0.052 0.404
#&gt; SRR1576845     2  0.6440     -0.639 0.008 0.536 0.052 0.404
#&gt; SRR1576846     2  0.6440     -0.639 0.008 0.536 0.052 0.404
#&gt; SRR1576847     4  0.7685      0.986 0.008 0.412 0.164 0.416
#&gt; SRR1576848     4  0.7685      0.986 0.008 0.412 0.164 0.416
#&gt; SRR1576849     4  0.7685      0.986 0.008 0.412 0.164 0.416
#&gt; SRR1576850     4  0.7685      0.986 0.008 0.412 0.164 0.416
#&gt; SRR1576851     4  0.7685      0.986 0.008 0.412 0.164 0.416
#&gt; SRR1576852     4  0.7685      0.986 0.008 0.412 0.164 0.416
#&gt; SRR1576853     3  0.1890      0.917 0.000 0.056 0.936 0.008
#&gt; SRR1576854     3  0.1890      0.917 0.000 0.056 0.936 0.008
#&gt; SRR1576855     3  0.1890      0.917 0.000 0.056 0.936 0.008
#&gt; SRR1576856     3  0.1890      0.917 0.000 0.056 0.936 0.008
#&gt; SRR1576857     1  0.6983      0.691 0.516 0.000 0.124 0.360
#&gt; SRR1576858     1  0.6983      0.691 0.516 0.000 0.124 0.360
#&gt; SRR1576859     1  0.6983      0.691 0.516 0.000 0.124 0.360
#&gt; SRR1576860     1  0.6983      0.691 0.516 0.000 0.124 0.360
#&gt; SRR1576861     1  0.6983      0.691 0.516 0.000 0.124 0.360
#&gt; SRR1576862     1  0.6983      0.691 0.516 0.000 0.124 0.360
#&gt; SRR1576863     2  0.6359      0.562 0.068 0.692 0.036 0.204
#&gt; SRR1576864     2  0.6359      0.562 0.068 0.692 0.036 0.204
#&gt; SRR1576865     2  0.6274      0.562 0.064 0.692 0.032 0.212
#&gt; SRR1576866     2  0.6274      0.562 0.064 0.692 0.032 0.212
#&gt; SRR1576867     1  0.3024      0.722 0.852 0.000 0.148 0.000
#&gt; SRR1576868     1  0.3024      0.722 0.852 0.000 0.148 0.000
#&gt; SRR1576869     1  0.3024      0.722 0.852 0.000 0.148 0.000
#&gt; SRR1576870     1  0.3080      0.741 0.880 0.000 0.096 0.024
#&gt; SRR1576871     1  0.3080      0.741 0.880 0.000 0.096 0.024
#&gt; SRR1576872     1  0.3080      0.741 0.880 0.000 0.096 0.024
#&gt; SRR1576873     2  0.3080      0.573 0.000 0.880 0.024 0.096
#&gt; SRR1576874     2  0.3080      0.573 0.000 0.880 0.024 0.096
#&gt; SRR1576875     2  0.3080      0.573 0.000 0.880 0.024 0.096
#&gt; SRR1576876     2  0.3080      0.573 0.000 0.880 0.024 0.096
#&gt; SRR1576877     1  0.5108      0.548 0.672 0.000 0.308 0.020
#&gt; SRR1576878     1  0.5108      0.548 0.672 0.000 0.308 0.020
#&gt; SRR1576879     1  0.5108      0.548 0.672 0.000 0.308 0.020
#&gt; SRR1576880     4  0.7477      0.971 0.004 0.416 0.152 0.428
#&gt; SRR1576881     4  0.7477      0.971 0.004 0.416 0.152 0.428
#&gt; SRR1576882     4  0.7477      0.971 0.004 0.416 0.152 0.428
#&gt; SRR1576883     2  0.2750      0.677 0.032 0.908 0.004 0.056
#&gt; SRR1576884     2  0.2750      0.677 0.032 0.908 0.004 0.056
#&gt; SRR1576885     2  0.2750      0.677 0.032 0.908 0.004 0.056
#&gt; SRR1576886     2  0.2750      0.677 0.032 0.908 0.004 0.056
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     4   0.510    -0.3381 0.004 0.380 0.016 0.588 0.012
#&gt; SRR1576834     4   0.510    -0.3381 0.004 0.380 0.016 0.588 0.012
#&gt; SRR1576835     4   0.511    -0.3482 0.004 0.384 0.016 0.584 0.012
#&gt; SRR1576836     4   0.511    -0.3482 0.004 0.384 0.016 0.584 0.012
#&gt; SRR1576837     5   0.120     0.9304 0.008 0.012 0.000 0.016 0.964
#&gt; SRR1576838     5   0.120     0.9304 0.008 0.012 0.000 0.016 0.964
#&gt; SRR1576839     5   0.117     0.9303 0.004 0.012 0.000 0.020 0.964
#&gt; SRR1576840     5   0.117     0.9303 0.004 0.012 0.000 0.020 0.964
#&gt; SRR1576841     5   0.330     0.8695 0.036 0.020 0.064 0.008 0.872
#&gt; SRR1576842     5   0.330     0.8695 0.036 0.020 0.064 0.008 0.872
#&gt; SRR1576843     5   0.330     0.8695 0.036 0.020 0.064 0.008 0.872
#&gt; SRR1576844     4   0.525     0.5418 0.028 0.016 0.276 0.668 0.012
#&gt; SRR1576845     4   0.525     0.5418 0.028 0.016 0.276 0.668 0.012
#&gt; SRR1576846     4   0.525     0.5418 0.028 0.016 0.276 0.668 0.012
#&gt; SRR1576847     4   0.507     0.5602 0.000 0.020 0.360 0.604 0.016
#&gt; SRR1576848     4   0.507     0.5602 0.000 0.020 0.360 0.604 0.016
#&gt; SRR1576849     4   0.507     0.5602 0.000 0.020 0.360 0.604 0.016
#&gt; SRR1576850     4   0.519     0.5612 0.000 0.024 0.372 0.588 0.016
#&gt; SRR1576851     4   0.519     0.5612 0.000 0.024 0.372 0.588 0.016
#&gt; SRR1576852     4   0.519     0.5612 0.000 0.024 0.372 0.588 0.016
#&gt; SRR1576853     5   0.222     0.9234 0.000 0.044 0.024 0.012 0.920
#&gt; SRR1576854     5   0.222     0.9234 0.000 0.044 0.024 0.012 0.920
#&gt; SRR1576855     5   0.220     0.9234 0.000 0.048 0.020 0.012 0.920
#&gt; SRR1576856     5   0.220     0.9234 0.000 0.048 0.020 0.012 0.920
#&gt; SRR1576857     1   0.586    -1.0000 0.452 0.000 0.452 0.000 0.096
#&gt; SRR1576858     3   0.586     0.0000 0.452 0.000 0.452 0.000 0.096
#&gt; SRR1576859     1   0.586    -1.0000 0.452 0.000 0.452 0.000 0.096
#&gt; SRR1576860     1   0.590    -0.9931 0.452 0.000 0.448 0.000 0.100
#&gt; SRR1576861     1   0.590    -0.9931 0.452 0.000 0.448 0.000 0.100
#&gt; SRR1576862     1   0.590    -0.9931 0.452 0.000 0.448 0.000 0.100
#&gt; SRR1576863     2   0.332     0.7079 0.000 0.844 0.036 0.116 0.004
#&gt; SRR1576864     2   0.332     0.7079 0.000 0.844 0.036 0.116 0.004
#&gt; SRR1576865     2   0.267     0.7079 0.000 0.872 0.008 0.116 0.004
#&gt; SRR1576866     2   0.267     0.7079 0.000 0.872 0.008 0.116 0.004
#&gt; SRR1576867     1   0.228     0.4454 0.880 0.000 0.000 0.000 0.120
#&gt; SRR1576868     1   0.228     0.4454 0.880 0.000 0.000 0.000 0.120
#&gt; SRR1576869     1   0.228     0.4454 0.880 0.000 0.000 0.000 0.120
#&gt; SRR1576870     1   0.277     0.3561 0.892 0.012 0.052 0.000 0.044
#&gt; SRR1576871     1   0.277     0.3561 0.892 0.012 0.052 0.000 0.044
#&gt; SRR1576872     1   0.277     0.3561 0.892 0.012 0.052 0.000 0.044
#&gt; SRR1576873     4   0.422    -0.0944 0.004 0.280 0.000 0.704 0.012
#&gt; SRR1576874     4   0.422    -0.0944 0.004 0.280 0.000 0.704 0.012
#&gt; SRR1576875     4   0.422    -0.0944 0.004 0.280 0.000 0.704 0.012
#&gt; SRR1576876     4   0.422    -0.0944 0.004 0.280 0.000 0.704 0.012
#&gt; SRR1576877     1   0.581     0.3930 0.648 0.044 0.048 0.004 0.256
#&gt; SRR1576878     1   0.581     0.3930 0.648 0.044 0.048 0.004 0.256
#&gt; SRR1576879     1   0.581     0.3930 0.648 0.044 0.048 0.004 0.256
#&gt; SRR1576880     4   0.510     0.5606 0.024 0.004 0.288 0.664 0.020
#&gt; SRR1576881     4   0.510     0.5606 0.024 0.004 0.288 0.664 0.020
#&gt; SRR1576882     4   0.510     0.5606 0.024 0.004 0.288 0.664 0.020
#&gt; SRR1576883     2   0.488     0.6583 0.004 0.564 0.004 0.416 0.012
#&gt; SRR1576884     2   0.488     0.6583 0.004 0.564 0.004 0.416 0.012
#&gt; SRR1576885     2   0.488     0.6583 0.004 0.564 0.004 0.416 0.012
#&gt; SRR1576886     2   0.488     0.6583 0.004 0.564 0.004 0.416 0.012
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.1109      0.731 0.000 0.964 0.016 0.004 0.004 0.012
#&gt; SRR1576834     2  0.1109      0.731 0.000 0.964 0.016 0.004 0.004 0.012
#&gt; SRR1576835     2  0.0964      0.729 0.000 0.968 0.016 0.000 0.004 0.012
#&gt; SRR1576836     2  0.0964      0.729 0.000 0.968 0.016 0.000 0.004 0.012
#&gt; SRR1576837     5  0.1010      0.900 0.000 0.036 0.000 0.004 0.960 0.000
#&gt; SRR1576838     5  0.1010      0.900 0.000 0.036 0.000 0.004 0.960 0.000
#&gt; SRR1576839     5  0.1010      0.900 0.000 0.036 0.000 0.004 0.960 0.000
#&gt; SRR1576840     5  0.1010      0.900 0.000 0.036 0.000 0.004 0.960 0.000
#&gt; SRR1576841     5  0.4326      0.797 0.028 0.000 0.072 0.020 0.788 0.092
#&gt; SRR1576842     5  0.4357      0.797 0.028 0.000 0.072 0.024 0.788 0.088
#&gt; SRR1576843     5  0.4326      0.797 0.028 0.000 0.072 0.020 0.788 0.092
#&gt; SRR1576844     4  0.6351      0.765 0.000 0.280 0.152 0.516 0.000 0.052
#&gt; SRR1576845     4  0.6351      0.765 0.000 0.280 0.152 0.516 0.000 0.052
#&gt; SRR1576846     4  0.6351      0.765 0.000 0.280 0.152 0.516 0.000 0.052
#&gt; SRR1576847     4  0.3756      0.842 0.000 0.184 0.024 0.776 0.012 0.004
#&gt; SRR1576848     4  0.3756      0.842 0.000 0.184 0.024 0.776 0.012 0.004
#&gt; SRR1576849     4  0.3756      0.842 0.000 0.184 0.024 0.776 0.012 0.004
#&gt; SRR1576850     4  0.3254      0.850 0.000 0.172 0.000 0.804 0.016 0.008
#&gt; SRR1576851     4  0.3254      0.850 0.000 0.172 0.000 0.804 0.016 0.008
#&gt; SRR1576852     4  0.3254      0.850 0.000 0.172 0.000 0.804 0.016 0.008
#&gt; SRR1576853     5  0.3379      0.887 0.000 0.036 0.020 0.048 0.856 0.040
#&gt; SRR1576854     5  0.3379      0.887 0.000 0.036 0.020 0.048 0.856 0.040
#&gt; SRR1576855     5  0.3361      0.887 0.000 0.036 0.016 0.048 0.856 0.044
#&gt; SRR1576856     5  0.3361      0.887 0.000 0.036 0.016 0.048 0.856 0.044
#&gt; SRR1576857     3  0.4481      0.976 0.296 0.000 0.648 0.000 0.056 0.000
#&gt; SRR1576858     3  0.4481      0.976 0.296 0.000 0.648 0.000 0.056 0.000
#&gt; SRR1576859     3  0.4481      0.976 0.296 0.000 0.648 0.000 0.056 0.000
#&gt; SRR1576860     3  0.5469      0.976 0.296 0.000 0.608 0.024 0.056 0.016
#&gt; SRR1576861     3  0.5469      0.976 0.296 0.000 0.608 0.024 0.056 0.016
#&gt; SRR1576862     3  0.5469      0.976 0.296 0.000 0.608 0.024 0.056 0.016
#&gt; SRR1576863     6  0.3922      0.974 0.000 0.320 0.000 0.016 0.000 0.664
#&gt; SRR1576864     6  0.3922      0.974 0.000 0.320 0.000 0.016 0.000 0.664
#&gt; SRR1576865     6  0.4985      0.974 0.008 0.320 0.012 0.024 0.012 0.624
#&gt; SRR1576866     6  0.4985      0.974 0.008 0.320 0.012 0.024 0.012 0.624
#&gt; SRR1576867     1  0.0865      0.781 0.964 0.000 0.000 0.000 0.036 0.000
#&gt; SRR1576868     1  0.0865      0.781 0.964 0.000 0.000 0.000 0.036 0.000
#&gt; SRR1576869     1  0.0865      0.781 0.964 0.000 0.000 0.000 0.036 0.000
#&gt; SRR1576870     1  0.2929      0.724 0.880 0.000 0.048 0.032 0.020 0.020
#&gt; SRR1576871     1  0.2929      0.724 0.880 0.000 0.048 0.032 0.020 0.020
#&gt; SRR1576872     1  0.2929      0.724 0.880 0.000 0.048 0.032 0.020 0.020
#&gt; SRR1576873     2  0.2706      0.712 0.008 0.880 0.040 0.068 0.004 0.000
#&gt; SRR1576874     2  0.2706      0.712 0.008 0.880 0.040 0.068 0.004 0.000
#&gt; SRR1576875     2  0.2706      0.712 0.008 0.880 0.040 0.068 0.004 0.000
#&gt; SRR1576876     2  0.2706      0.712 0.008 0.880 0.040 0.068 0.004 0.000
#&gt; SRR1576877     1  0.5925      0.674 0.660 0.000 0.036 0.044 0.136 0.124
#&gt; SRR1576878     1  0.5946      0.674 0.660 0.000 0.036 0.048 0.136 0.120
#&gt; SRR1576879     1  0.5946      0.674 0.660 0.000 0.036 0.048 0.136 0.120
#&gt; SRR1576880     4  0.5773      0.839 0.000 0.200 0.108 0.636 0.008 0.048
#&gt; SRR1576881     4  0.5773      0.839 0.000 0.200 0.108 0.636 0.008 0.048
#&gt; SRR1576882     4  0.5773      0.839 0.000 0.200 0.108 0.636 0.008 0.048
#&gt; SRR1576883     2  0.4470      0.395 0.000 0.684 0.052 0.008 0.000 0.256
#&gt; SRR1576884     2  0.4470      0.395 0.000 0.684 0.052 0.008 0.000 0.256
#&gt; SRR1576885     2  0.4470      0.395 0.000 0.684 0.052 0.008 0.000 0.256
#&gt; SRR1576886     2  0.4470      0.395 0.000 0.684 0.052 0.008 0.000 0.256
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.5092 0.491   0.491
#> 3 3 1.000           0.991       0.990         0.1983 0.899   0.795
#> 4 4 0.807           0.966       0.940         0.2005 0.866   0.657
#> 5 5 0.910           0.946       0.911         0.0747 0.943   0.779
#> 6 6 0.862           0.800       0.827         0.0428 0.981   0.906
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1576833     2       0          1  0  1
#&gt; SRR1576834     2       0          1  0  1
#&gt; SRR1576835     2       0          1  0  1
#&gt; SRR1576836     2       0          1  0  1
#&gt; SRR1576837     1       0          1  1  0
#&gt; SRR1576838     1       0          1  1  0
#&gt; SRR1576839     1       0          1  1  0
#&gt; SRR1576840     1       0          1  1  0
#&gt; SRR1576841     1       0          1  1  0
#&gt; SRR1576842     1       0          1  1  0
#&gt; SRR1576843     1       0          1  1  0
#&gt; SRR1576844     2       0          1  0  1
#&gt; SRR1576845     2       0          1  0  1
#&gt; SRR1576846     2       0          1  0  1
#&gt; SRR1576847     2       0          1  0  1
#&gt; SRR1576848     2       0          1  0  1
#&gt; SRR1576849     2       0          1  0  1
#&gt; SRR1576850     2       0          1  0  1
#&gt; SRR1576851     2       0          1  0  1
#&gt; SRR1576852     2       0          1  0  1
#&gt; SRR1576853     1       0          1  1  0
#&gt; SRR1576854     1       0          1  1  0
#&gt; SRR1576855     1       0          1  1  0
#&gt; SRR1576856     1       0          1  1  0
#&gt; SRR1576857     1       0          1  1  0
#&gt; SRR1576858     1       0          1  1  0
#&gt; SRR1576859     1       0          1  1  0
#&gt; SRR1576860     1       0          1  1  0
#&gt; SRR1576861     1       0          1  1  0
#&gt; SRR1576862     1       0          1  1  0
#&gt; SRR1576863     2       0          1  0  1
#&gt; SRR1576864     2       0          1  0  1
#&gt; SRR1576865     2       0          1  0  1
#&gt; SRR1576866     2       0          1  0  1
#&gt; SRR1576867     1       0          1  1  0
#&gt; SRR1576868     1       0          1  1  0
#&gt; SRR1576869     1       0          1  1  0
#&gt; SRR1576870     1       0          1  1  0
#&gt; SRR1576871     1       0          1  1  0
#&gt; SRR1576872     1       0          1  1  0
#&gt; SRR1576873     2       0          1  0  1
#&gt; SRR1576874     2       0          1  0  1
#&gt; SRR1576875     2       0          1  0  1
#&gt; SRR1576876     2       0          1  0  1
#&gt; SRR1576877     1       0          1  1  0
#&gt; SRR1576878     1       0          1  1  0
#&gt; SRR1576879     1       0          1  1  0
#&gt; SRR1576880     2       0          1  0  1
#&gt; SRR1576881     2       0          1  0  1
#&gt; SRR1576882     2       0          1  0  1
#&gt; SRR1576883     2       0          1  0  1
#&gt; SRR1576884     2       0          1  0  1
#&gt; SRR1576885     2       0          1  0  1
#&gt; SRR1576886     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576837     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576838     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576839     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576840     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576841     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576842     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576843     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576844     2  0.0424      0.995 0.008 0.992 0.000
#&gt; SRR1576845     2  0.0424      0.995 0.008 0.992 0.000
#&gt; SRR1576846     2  0.0424      0.995 0.008 0.992 0.000
#&gt; SRR1576847     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576848     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576849     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576850     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576851     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576852     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576853     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576854     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576855     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576856     1  0.0592      1.000 0.988 0.000 0.012
#&gt; SRR1576857     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576858     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576859     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576860     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576861     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576862     3  0.1411      0.980 0.036 0.000 0.964
#&gt; SRR1576863     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576864     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576865     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576866     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576867     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576868     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576869     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576870     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576871     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576872     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576873     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576877     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576878     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576879     3  0.0000      0.980 0.000 0.000 1.000
#&gt; SRR1576880     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576881     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576882     2  0.0592      0.994 0.012 0.988 0.000
#&gt; SRR1576883     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576884     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576885     2  0.0000      0.996 0.000 1.000 0.000
#&gt; SRR1576886     2  0.0000      0.996 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576837     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; SRR1576838     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; SRR1576839     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; SRR1576840     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; SRR1576841     3  0.1118      0.923 0.036 0.000 0.964 0.000
#&gt; SRR1576842     3  0.1118      0.923 0.036 0.000 0.964 0.000
#&gt; SRR1576843     3  0.1118      0.923 0.036 0.000 0.964 0.000
#&gt; SRR1576844     4  0.3688      0.923 0.000 0.208 0.000 0.792
#&gt; SRR1576845     4  0.3688      0.923 0.000 0.208 0.000 0.792
#&gt; SRR1576846     4  0.3688      0.923 0.000 0.208 0.000 0.792
#&gt; SRR1576847     4  0.2760      0.971 0.000 0.128 0.000 0.872
#&gt; SRR1576848     4  0.2760      0.971 0.000 0.128 0.000 0.872
#&gt; SRR1576849     4  0.2760      0.971 0.000 0.128 0.000 0.872
#&gt; SRR1576850     4  0.2760      0.971 0.000 0.128 0.000 0.872
#&gt; SRR1576851     4  0.2760      0.971 0.000 0.128 0.000 0.872
#&gt; SRR1576852     4  0.2760      0.971 0.000 0.128 0.000 0.872
#&gt; SRR1576853     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576854     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576855     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576856     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576857     3  0.1022      0.924 0.032 0.000 0.968 0.000
#&gt; SRR1576858     3  0.1022      0.924 0.032 0.000 0.968 0.000
#&gt; SRR1576859     3  0.1022      0.924 0.032 0.000 0.968 0.000
#&gt; SRR1576860     3  0.1022      0.924 0.032 0.000 0.968 0.000
#&gt; SRR1576861     3  0.1022      0.924 0.032 0.000 0.968 0.000
#&gt; SRR1576862     3  0.1022      0.924 0.032 0.000 0.968 0.000
#&gt; SRR1576863     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576864     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576865     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576866     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576867     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576868     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576869     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576870     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576871     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576872     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576873     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576877     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576878     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576879     3  0.2704      0.926 0.000 0.000 0.876 0.124
#&gt; SRR1576880     4  0.2921      0.970 0.000 0.140 0.000 0.860
#&gt; SRR1576881     4  0.2921      0.970 0.000 0.140 0.000 0.860
#&gt; SRR1576882     4  0.2921      0.970 0.000 0.140 0.000 0.860
#&gt; SRR1576883     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      1.000 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576834     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576835     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576836     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576837     5  0.0963      0.947 0.000 0.000 0.036 0.000 0.964
#&gt; SRR1576838     5  0.0963      0.947 0.000 0.000 0.036 0.000 0.964
#&gt; SRR1576839     5  0.0963      0.947 0.000 0.000 0.036 0.000 0.964
#&gt; SRR1576840     5  0.0963      0.947 0.000 0.000 0.036 0.000 0.964
#&gt; SRR1576841     3  0.3366      0.958 0.232 0.000 0.768 0.000 0.000
#&gt; SRR1576842     3  0.3366      0.958 0.232 0.000 0.768 0.000 0.000
#&gt; SRR1576843     3  0.3366      0.958 0.232 0.000 0.768 0.000 0.000
#&gt; SRR1576844     4  0.2966      0.794 0.000 0.184 0.000 0.816 0.000
#&gt; SRR1576845     4  0.2966      0.794 0.000 0.184 0.000 0.816 0.000
#&gt; SRR1576846     4  0.2966      0.794 0.000 0.184 0.000 0.816 0.000
#&gt; SRR1576847     4  0.2563      0.880 0.000 0.008 0.120 0.872 0.000
#&gt; SRR1576848     4  0.2563      0.880 0.000 0.008 0.120 0.872 0.000
#&gt; SRR1576849     4  0.2563      0.880 0.000 0.008 0.120 0.872 0.000
#&gt; SRR1576850     4  0.2563      0.880 0.000 0.008 0.120 0.872 0.000
#&gt; SRR1576851     4  0.2563      0.880 0.000 0.008 0.120 0.872 0.000
#&gt; SRR1576852     4  0.2563      0.880 0.000 0.008 0.120 0.872 0.000
#&gt; SRR1576853     5  0.2077      0.947 0.000 0.000 0.084 0.008 0.908
#&gt; SRR1576854     5  0.2077      0.947 0.000 0.000 0.084 0.008 0.908
#&gt; SRR1576855     5  0.2077      0.947 0.000 0.000 0.084 0.008 0.908
#&gt; SRR1576856     5  0.2077      0.947 0.000 0.000 0.084 0.008 0.908
#&gt; SRR1576857     3  0.3636      0.979 0.272 0.000 0.728 0.000 0.000
#&gt; SRR1576858     3  0.3636      0.979 0.272 0.000 0.728 0.000 0.000
#&gt; SRR1576859     3  0.3636      0.979 0.272 0.000 0.728 0.000 0.000
#&gt; SRR1576860     3  0.3636      0.979 0.272 0.000 0.728 0.000 0.000
#&gt; SRR1576861     3  0.3636      0.979 0.272 0.000 0.728 0.000 0.000
#&gt; SRR1576862     3  0.3636      0.979 0.272 0.000 0.728 0.000 0.000
#&gt; SRR1576863     2  0.1904      0.965 0.000 0.936 0.020 0.028 0.016
#&gt; SRR1576864     2  0.1904      0.965 0.000 0.936 0.020 0.028 0.016
#&gt; SRR1576865     2  0.1904      0.965 0.000 0.936 0.020 0.028 0.016
#&gt; SRR1576866     2  0.1904      0.965 0.000 0.936 0.020 0.028 0.016
#&gt; SRR1576867     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0162      0.996 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1576871     1  0.0162      0.996 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1576872     1  0.0162      0.996 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1576873     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576874     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576875     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576876     2  0.0162      0.973 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1576877     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.1430      0.870 0.000 0.052 0.004 0.944 0.000
#&gt; SRR1576881     4  0.1430      0.870 0.000 0.052 0.004 0.944 0.000
#&gt; SRR1576882     4  0.1430      0.870 0.000 0.052 0.004 0.944 0.000
#&gt; SRR1576883     2  0.1310      0.971 0.000 0.956 0.020 0.024 0.000
#&gt; SRR1576884     2  0.1310      0.971 0.000 0.956 0.020 0.024 0.000
#&gt; SRR1576885     2  0.1310      0.971 0.000 0.956 0.020 0.024 0.000
#&gt; SRR1576886     2  0.1310      0.971 0.000 0.956 0.020 0.024 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.3003      0.869 0.000 0.812 0.016 0.000 0.000 0.172
#&gt; SRR1576834     2  0.3003      0.869 0.000 0.812 0.016 0.000 0.000 0.172
#&gt; SRR1576835     2  0.3003      0.869 0.000 0.812 0.016 0.000 0.000 0.172
#&gt; SRR1576836     2  0.3003      0.869 0.000 0.812 0.016 0.000 0.000 0.172
#&gt; SRR1576837     5  0.0146      0.869 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1576838     5  0.0146      0.869 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1576839     5  0.0146      0.869 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1576840     5  0.0146      0.869 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1576841     3  0.2629      0.941 0.068 0.000 0.872 0.000 0.000 0.060
#&gt; SRR1576842     3  0.2629      0.941 0.068 0.000 0.872 0.000 0.000 0.060
#&gt; SRR1576843     3  0.2629      0.941 0.068 0.000 0.872 0.000 0.000 0.060
#&gt; SRR1576844     6  0.5190      1.000 0.000 0.088 0.000 0.452 0.000 0.460
#&gt; SRR1576845     6  0.5190      1.000 0.000 0.088 0.000 0.452 0.000 0.460
#&gt; SRR1576846     6  0.5190      1.000 0.000 0.088 0.000 0.452 0.000 0.460
#&gt; SRR1576847     4  0.0000      0.660 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576848     4  0.0000      0.660 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576849     4  0.0000      0.660 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576850     4  0.0000      0.660 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576851     4  0.0000      0.660 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576852     4  0.0000      0.660 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576853     5  0.4075      0.869 0.000 0.000 0.048 0.000 0.712 0.240
#&gt; SRR1576854     5  0.4075      0.869 0.000 0.000 0.048 0.000 0.712 0.240
#&gt; SRR1576855     5  0.4075      0.869 0.000 0.000 0.048 0.000 0.712 0.240
#&gt; SRR1576856     5  0.4075      0.869 0.000 0.000 0.048 0.000 0.712 0.240
#&gt; SRR1576857     3  0.1814      0.971 0.100 0.000 0.900 0.000 0.000 0.000
#&gt; SRR1576858     3  0.1814      0.971 0.100 0.000 0.900 0.000 0.000 0.000
#&gt; SRR1576859     3  0.1814      0.971 0.100 0.000 0.900 0.000 0.000 0.000
#&gt; SRR1576860     3  0.1814      0.971 0.100 0.000 0.900 0.000 0.000 0.000
#&gt; SRR1576861     3  0.1814      0.971 0.100 0.000 0.900 0.000 0.000 0.000
#&gt; SRR1576862     3  0.1814      0.971 0.100 0.000 0.900 0.000 0.000 0.000
#&gt; SRR1576863     2  0.1806      0.838 0.000 0.908 0.004 0.000 0.000 0.088
#&gt; SRR1576864     2  0.1806      0.838 0.000 0.908 0.004 0.000 0.000 0.088
#&gt; SRR1576865     2  0.1806      0.838 0.000 0.908 0.004 0.000 0.000 0.088
#&gt; SRR1576866     2  0.1806      0.838 0.000 0.908 0.004 0.000 0.000 0.088
#&gt; SRR1576867     1  0.0146      0.993 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0146      0.993 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0146      0.993 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0363      0.990 0.988 0.000 0.012 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0363      0.990 0.988 0.000 0.012 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0363      0.990 0.988 0.000 0.012 0.000 0.000 0.000
#&gt; SRR1576873     2  0.3071      0.867 0.000 0.804 0.016 0.000 0.000 0.180
#&gt; SRR1576874     2  0.3071      0.867 0.000 0.804 0.016 0.000 0.000 0.180
#&gt; SRR1576875     2  0.3071      0.867 0.000 0.804 0.016 0.000 0.000 0.180
#&gt; SRR1576876     2  0.3071      0.867 0.000 0.804 0.016 0.000 0.000 0.180
#&gt; SRR1576877     1  0.0260      0.990 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; SRR1576878     1  0.0260      0.990 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; SRR1576879     1  0.0260      0.990 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; SRR1576880     4  0.3995     -0.675 0.000 0.004 0.000 0.516 0.000 0.480
#&gt; SRR1576881     4  0.3995     -0.675 0.000 0.004 0.000 0.516 0.000 0.480
#&gt; SRR1576882     4  0.3995     -0.675 0.000 0.004 0.000 0.516 0.000 0.480
#&gt; SRR1576883     2  0.0865      0.860 0.000 0.964 0.000 0.000 0.000 0.036
#&gt; SRR1576884     2  0.0865      0.860 0.000 0.964 0.000 0.000 0.000 0.036
#&gt; SRR1576885     2  0.0865      0.860 0.000 0.964 0.000 0.000 0.000 0.036
#&gt; SRR1576886     2  0.0865      0.860 0.000 0.964 0.000 0.000 0.000 0.036
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.921       0.974         0.4312 0.575   0.575
#> 3 3 0.730           0.853       0.914         0.3861 0.711   0.538
#> 4 4 1.000           0.977       0.988         0.1173 0.929   0.820
#> 5 5 0.866           0.857       0.908         0.1329 0.933   0.802
#> 6 6 1.000           0.957       0.976         0.0927 0.899   0.629
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 4
```

There is also optional best $k$ = 2 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576834     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576835     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576836     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576837     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576838     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576839     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576840     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576841     1   0.997     0.0704 0.532 0.468
#&gt; SRR1576842     2   1.000     0.0140 0.488 0.512
#&gt; SRR1576843     2   0.995     0.1168 0.460 0.540
#&gt; SRR1576844     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576845     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576846     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576847     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576848     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576849     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576850     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576851     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576852     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576853     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576854     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576855     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576856     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576857     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576858     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576859     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576860     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576861     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576862     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576863     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576864     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576865     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576866     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576867     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576868     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576869     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576870     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576871     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576872     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576873     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576874     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576875     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576876     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576877     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576878     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576879     1   0.000     0.9671 1.000 0.000
#&gt; SRR1576880     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576881     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576882     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576883     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576884     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576885     2   0.000     0.9729 0.000 1.000
#&gt; SRR1576886     2   0.000     0.9729 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576834     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576835     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576836     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576837     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576838     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576839     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576840     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576841     1   0.470      0.622 0.788 0.000 0.212
#&gt; SRR1576842     1   0.470      0.622 0.788 0.000 0.212
#&gt; SRR1576843     1   0.470      0.622 0.788 0.000 0.212
#&gt; SRR1576844     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576845     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576846     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576847     2   0.196      0.938 0.056 0.944 0.000
#&gt; SRR1576848     2   0.196      0.938 0.056 0.944 0.000
#&gt; SRR1576849     2   0.196      0.938 0.056 0.944 0.000
#&gt; SRR1576850     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576851     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576852     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576853     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576854     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576855     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576856     1   0.470      0.734 0.788 0.212 0.000
#&gt; SRR1576857     3   0.000      0.910 0.000 0.000 1.000
#&gt; SRR1576858     3   0.000      0.910 0.000 0.000 1.000
#&gt; SRR1576859     3   0.000      0.910 0.000 0.000 1.000
#&gt; SRR1576860     3   0.000      0.910 0.000 0.000 1.000
#&gt; SRR1576861     3   0.000      0.910 0.000 0.000 1.000
#&gt; SRR1576862     3   0.000      0.910 0.000 0.000 1.000
#&gt; SRR1576863     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576864     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576865     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576866     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576867     1   0.553      0.400 0.704 0.000 0.296
#&gt; SRR1576868     1   0.556      0.392 0.700 0.000 0.300
#&gt; SRR1576869     1   0.510      0.482 0.752 0.000 0.248
#&gt; SRR1576870     3   0.470      0.812 0.212 0.000 0.788
#&gt; SRR1576871     3   0.470      0.812 0.212 0.000 0.788
#&gt; SRR1576872     3   0.470      0.812 0.212 0.000 0.788
#&gt; SRR1576873     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576874     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576875     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576876     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576877     1   0.489      0.510 0.772 0.000 0.228
#&gt; SRR1576878     1   0.489      0.510 0.772 0.000 0.228
#&gt; SRR1576879     1   0.489      0.510 0.772 0.000 0.228
#&gt; SRR1576880     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576881     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576882     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576883     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576884     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576885     2   0.000      0.993 0.000 1.000 0.000
#&gt; SRR1576886     2   0.000      0.993 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576837     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576838     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576839     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576840     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576841     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576842     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576843     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576844     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576845     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576846     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576847     2  0.3626      0.790 0.004 0.812 0.000 0.184
#&gt; SRR1576848     2  0.3626      0.790 0.004 0.812 0.000 0.184
#&gt; SRR1576849     2  0.3626      0.790 0.004 0.812 0.000 0.184
#&gt; SRR1576850     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576851     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576852     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576853     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576854     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576855     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576856     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; SRR1576857     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1576858     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1576859     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1576860     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1576861     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1576862     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1576863     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576864     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576865     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576866     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576867     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576868     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576869     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576870     1  0.0188      0.996 0.996 0.000 0.004 0.000
#&gt; SRR1576871     1  0.0188      0.996 0.996 0.000 0.004 0.000
#&gt; SRR1576872     1  0.0188      0.996 0.996 0.000 0.004 0.000
#&gt; SRR1576873     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576877     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576878     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576879     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; SRR1576880     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576881     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576882     2  0.0188      0.976 0.004 0.996 0.000 0.000
#&gt; SRR1576883     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576884     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576885     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; SRR1576886     2  0.0000      0.976 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4 p5
#&gt; SRR1576833     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576834     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576835     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576836     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576837     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576838     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576839     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576840     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576841     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576842     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576843     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576844     2  0.0290      0.718 0.008 0.992 0.000 0.000  0
#&gt; SRR1576845     2  0.0609      0.717 0.020 0.980 0.000 0.000  0
#&gt; SRR1576846     2  0.0510      0.717 0.016 0.984 0.000 0.000  0
#&gt; SRR1576847     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576848     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576849     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576850     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576851     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576852     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576853     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576854     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576855     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576856     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1576857     3  0.4210      1.000 0.000 0.000 0.588 0.412  0
#&gt; SRR1576858     3  0.4210      1.000 0.000 0.000 0.588 0.412  0
#&gt; SRR1576859     3  0.4210      1.000 0.000 0.000 0.588 0.412  0
#&gt; SRR1576860     3  0.4210      1.000 0.000 0.000 0.588 0.412  0
#&gt; SRR1576861     3  0.4210      1.000 0.000 0.000 0.588 0.412  0
#&gt; SRR1576862     3  0.4210      1.000 0.000 0.000 0.588 0.412  0
#&gt; SRR1576863     4  0.4210      1.000 0.412 0.000 0.000 0.588  0
#&gt; SRR1576864     4  0.4210      1.000 0.412 0.000 0.000 0.588  0
#&gt; SRR1576865     4  0.4210      1.000 0.412 0.000 0.000 0.588  0
#&gt; SRR1576866     4  0.4210      1.000 0.412 0.000 0.000 0.588  0
#&gt; SRR1576867     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576868     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576869     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576870     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576871     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576872     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576873     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576874     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576875     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576876     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576877     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576878     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576879     1  0.4210      1.000 0.588 0.000 0.412 0.000  0
#&gt; SRR1576880     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576881     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576882     2  0.0000      0.717 0.000 1.000 0.000 0.000  0
#&gt; SRR1576883     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576884     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576885     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
#&gt; SRR1576886     2  0.4210      0.640 0.412 0.588 0.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4 p5    p6
#&gt; SRR1576833     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576834     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576835     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576836     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576837     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576838     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576839     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576840     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576841     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576842     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576843     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576844     4   0.350      0.614 0.000 0.320  0 0.680  0 0.000
#&gt; SRR1576845     4   0.352      0.607 0.000 0.324  0 0.676  0 0.000
#&gt; SRR1576846     4   0.341      0.642 0.000 0.300  0 0.700  0 0.000
#&gt; SRR1576847     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576848     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576849     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576850     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576851     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576852     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576853     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576854     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576855     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576856     5   0.000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; SRR1576857     3   0.000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; SRR1576858     3   0.000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; SRR1576859     3   0.000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; SRR1576860     3   0.000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; SRR1576861     3   0.000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; SRR1576862     3   0.000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; SRR1576863     6   0.114      1.000 0.000 0.052  0 0.000  0 0.948
#&gt; SRR1576864     6   0.114      1.000 0.000 0.052  0 0.000  0 0.948
#&gt; SRR1576865     6   0.114      1.000 0.000 0.052  0 0.000  0 0.948
#&gt; SRR1576866     6   0.114      1.000 0.000 0.052  0 0.000  0 0.948
#&gt; SRR1576867     1   0.000      0.984 1.000 0.000  0 0.000  0 0.000
#&gt; SRR1576868     1   0.000      0.984 1.000 0.000  0 0.000  0 0.000
#&gt; SRR1576869     1   0.000      0.984 1.000 0.000  0 0.000  0 0.000
#&gt; SRR1576870     1   0.000      0.984 1.000 0.000  0 0.000  0 0.000
#&gt; SRR1576871     1   0.000      0.984 1.000 0.000  0 0.000  0 0.000
#&gt; SRR1576872     1   0.000      0.984 1.000 0.000  0 0.000  0 0.000
#&gt; SRR1576873     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576874     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576875     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576876     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576877     1   0.114      0.967 0.948 0.000  0 0.000  0 0.052
#&gt; SRR1576878     1   0.114      0.967 0.948 0.000  0 0.000  0 0.052
#&gt; SRR1576879     1   0.114      0.967 0.948 0.000  0 0.000  0 0.052
#&gt; SRR1576880     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576881     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576882     4   0.000      0.890 0.000 0.000  0 1.000  0 0.000
#&gt; SRR1576883     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576884     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576885     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
#&gt; SRR1576886     2   0.000      1.000 0.000 1.000  0 0.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.958       0.967         0.4149 0.591   0.591
#> 3 3 0.676           0.905       0.911         0.3843 0.623   0.464
#> 4 4 0.831           0.925       0.933         0.2426 0.849   0.657
#> 5 5 1.000           0.993       0.996         0.1364 0.899   0.652
#> 6 6 1.000           0.998       0.999         0.0282 0.978   0.881
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576834     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576835     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576836     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576837     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576838     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576839     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576840     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576841     2  0.5842      0.835 0.140 0.860
#&gt; SRR1576842     2  0.5408      0.855 0.124 0.876
#&gt; SRR1576843     2  0.6148      0.820 0.152 0.848
#&gt; SRR1576844     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576845     2  0.3274      0.947 0.060 0.940
#&gt; SRR1576846     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576847     2  0.2043      0.959 0.032 0.968
#&gt; SRR1576848     2  0.2043      0.959 0.032 0.968
#&gt; SRR1576849     2  0.2043      0.959 0.032 0.968
#&gt; SRR1576850     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576851     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576852     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576853     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576854     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576855     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576856     2  0.0376      0.969 0.004 0.996
#&gt; SRR1576857     1  0.4022      0.961 0.920 0.080
#&gt; SRR1576858     1  0.4022      0.961 0.920 0.080
#&gt; SRR1576859     1  0.4022      0.961 0.920 0.080
#&gt; SRR1576860     1  0.4022      0.961 0.920 0.080
#&gt; SRR1576861     1  0.4022      0.961 0.920 0.080
#&gt; SRR1576862     1  0.4022      0.961 0.920 0.080
#&gt; SRR1576863     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576864     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576865     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576866     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576867     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576868     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576869     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576870     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576871     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576872     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576873     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576874     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576875     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576876     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576877     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576878     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576879     1  0.1414      0.975 0.980 0.020
#&gt; SRR1576880     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576881     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576882     2  0.3431      0.945 0.064 0.936
#&gt; SRR1576883     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576884     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576885     2  0.0000      0.970 0.000 1.000
#&gt; SRR1576886     2  0.0000      0.970 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576834     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576835     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576836     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576837     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576838     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576839     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576840     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576841     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576842     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576843     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576844     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576845     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576846     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576847     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576848     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576849     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576850     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576851     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576852     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576853     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576854     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576855     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576856     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576857     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576858     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576859     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576860     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576861     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576862     2   0.245      0.861 0.076 0.924 0.000
#&gt; SRR1576863     2   0.675      0.836 0.076 0.732 0.192
#&gt; SRR1576864     2   0.675      0.836 0.076 0.732 0.192
#&gt; SRR1576865     2   0.675      0.836 0.076 0.732 0.192
#&gt; SRR1576866     2   0.675      0.836 0.076 0.732 0.192
#&gt; SRR1576867     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576868     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576869     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576870     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576871     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576872     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576873     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576874     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576875     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576876     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576877     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576878     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576879     1   0.000      1.000 1.000 0.000 0.000
#&gt; SRR1576880     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576881     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576882     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1576883     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576884     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576885     2   0.445      0.822 0.000 0.808 0.192
#&gt; SRR1576886     2   0.445      0.822 0.000 0.808 0.192
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1576833     2  0.3895      0.830  0 0.804 0.012 0.184
#&gt; SRR1576834     2  0.4012      0.829  0 0.800 0.016 0.184
#&gt; SRR1576835     2  0.3895      0.830  0 0.804 0.012 0.184
#&gt; SRR1576836     2  0.3810      0.829  0 0.804 0.008 0.188
#&gt; SRR1576837     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576838     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576839     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576840     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576841     3  0.0592      0.984  0 0.016 0.984 0.000
#&gt; SRR1576842     3  0.0592      0.984  0 0.016 0.984 0.000
#&gt; SRR1576843     3  0.0592      0.984  0 0.016 0.984 0.000
#&gt; SRR1576844     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576845     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576846     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576847     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576848     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576849     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576850     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576851     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576852     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576853     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576854     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576855     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576856     2  0.2011      0.842  0 0.920 0.080 0.000
#&gt; SRR1576857     3  0.0000      0.992  0 0.000 1.000 0.000
#&gt; SRR1576858     3  0.0000      0.992  0 0.000 1.000 0.000
#&gt; SRR1576859     3  0.0000      0.992  0 0.000 1.000 0.000
#&gt; SRR1576860     3  0.0000      0.992  0 0.000 1.000 0.000
#&gt; SRR1576861     3  0.0000      0.992  0 0.000 1.000 0.000
#&gt; SRR1576862     3  0.0000      0.992  0 0.000 1.000 0.000
#&gt; SRR1576863     2  0.2197      0.843  0 0.916 0.080 0.004
#&gt; SRR1576864     2  0.2197      0.843  0 0.916 0.080 0.004
#&gt; SRR1576865     2  0.2197      0.843  0 0.916 0.080 0.004
#&gt; SRR1576866     2  0.2197      0.843  0 0.916 0.080 0.004
#&gt; SRR1576867     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576873     2  0.3969      0.832  0 0.804 0.016 0.180
#&gt; SRR1576874     2  0.4079      0.830  0 0.800 0.020 0.180
#&gt; SRR1576875     2  0.4499      0.825  0 0.792 0.048 0.160
#&gt; SRR1576876     2  0.4332      0.830  0 0.800 0.040 0.160
#&gt; SRR1576877     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576881     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576882     4  0.0000      1.000  0 0.000 0.000 1.000
#&gt; SRR1576883     2  0.3486      0.830  0 0.812 0.000 0.188
#&gt; SRR1576884     2  0.3486      0.830  0 0.812 0.000 0.188
#&gt; SRR1576885     2  0.3486      0.830  0 0.812 0.000 0.188
#&gt; SRR1576886     2  0.3486      0.830  0 0.812 0.000 0.188
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3 p4    p5
#&gt; SRR1576833     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576834     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576835     2  0.0290      0.992  0 0.992 0.008  0 0.000
#&gt; SRR1576836     2  0.0290      0.992  0 0.992 0.008  0 0.000
#&gt; SRR1576837     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576838     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576839     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576840     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576841     3  0.0290      0.994  0 0.000 0.992  0 0.008
#&gt; SRR1576842     3  0.0290      0.994  0 0.000 0.992  0 0.008
#&gt; SRR1576843     3  0.0290      0.994  0 0.000 0.992  0 0.008
#&gt; SRR1576844     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576845     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576846     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576847     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576848     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576849     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576850     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576851     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576852     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576853     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576854     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576855     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576856     5  0.0000      0.983  0 0.000 0.000  0 1.000
#&gt; SRR1576857     3  0.0000      0.997  0 0.000 1.000  0 0.000
#&gt; SRR1576858     3  0.0000      0.997  0 0.000 1.000  0 0.000
#&gt; SRR1576859     3  0.0000      0.997  0 0.000 1.000  0 0.000
#&gt; SRR1576860     3  0.0000      0.997  0 0.000 1.000  0 0.000
#&gt; SRR1576861     3  0.0000      0.997  0 0.000 1.000  0 0.000
#&gt; SRR1576862     3  0.0000      0.997  0 0.000 1.000  0 0.000
#&gt; SRR1576863     5  0.1121      0.966  0 0.044 0.000  0 0.956
#&gt; SRR1576864     5  0.1121      0.966  0 0.044 0.000  0 0.956
#&gt; SRR1576865     5  0.1121      0.966  0 0.044 0.000  0 0.956
#&gt; SRR1576866     5  0.1121      0.966  0 0.044 0.000  0 0.956
#&gt; SRR1576867     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576868     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576869     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576870     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576871     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576872     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576873     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576874     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576875     2  0.0162      0.995  0 0.996 0.000  0 0.004
#&gt; SRR1576876     2  0.0162      0.995  0 0.996 0.000  0 0.004
#&gt; SRR1576877     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576878     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576879     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; SRR1576880     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576881     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576882     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; SRR1576883     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576884     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576885     2  0.0000      0.998  0 1.000 0.000  0 0.000
#&gt; SRR1576886     2  0.0000      0.998  0 1.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576834     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576835     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576836     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576837     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576838     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576839     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576840     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576841     3  0.0260      0.993  0 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1576842     3  0.0260      0.993  0 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1576843     3  0.0260      0.993  0 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1576844     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576845     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576846     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576847     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576848     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576849     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576850     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576851     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576852     4  0.0000      0.999  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576853     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576854     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576855     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576856     5  0.0000      1.000  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1576857     3  0.0000      0.997  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576858     3  0.0000      0.997  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.997  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576860     3  0.0000      0.997  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576861     3  0.0000      0.997  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576862     3  0.0000      0.997  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1576863     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576864     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576865     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576866     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1576867     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576868     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576869     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576870     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576871     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576872     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576873     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576874     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576875     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576876     2  0.0000      0.998  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1576877     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576878     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576879     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1576880     4  0.0146      0.996  0 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1576881     4  0.0146      0.996  0 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1576882     4  0.0146      0.996  0 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1576883     2  0.0146      0.997  0 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576884     2  0.0146      0.997  0 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576885     2  0.0146      0.997  0 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1576886     2  0.0146      0.997  0 0.996 0.000 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15448 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.961           0.984       0.991         0.5074 0.491   0.491
#> 3 3 0.823           0.915       0.915         0.2121 0.893   0.782
#> 4 4 0.606           0.686       0.821         0.1656 0.829   0.581
#> 5 5 0.753           0.876       0.869         0.0899 0.888   0.602
#> 6 6 0.819           0.737       0.783         0.0518 0.915   0.643
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1576833     2   0.000      1.000 0.000 1.000
#&gt; SRR1576834     2   0.000      1.000 0.000 1.000
#&gt; SRR1576835     2   0.000      1.000 0.000 1.000
#&gt; SRR1576836     2   0.000      1.000 0.000 1.000
#&gt; SRR1576837     1   0.000      0.981 1.000 0.000
#&gt; SRR1576838     1   0.000      0.981 1.000 0.000
#&gt; SRR1576839     1   0.000      0.981 1.000 0.000
#&gt; SRR1576840     1   0.000      0.981 1.000 0.000
#&gt; SRR1576841     1   0.000      0.981 1.000 0.000
#&gt; SRR1576842     1   0.000      0.981 1.000 0.000
#&gt; SRR1576843     1   0.000      0.981 1.000 0.000
#&gt; SRR1576844     2   0.000      1.000 0.000 1.000
#&gt; SRR1576845     2   0.000      1.000 0.000 1.000
#&gt; SRR1576846     2   0.000      1.000 0.000 1.000
#&gt; SRR1576847     2   0.000      1.000 0.000 1.000
#&gt; SRR1576848     2   0.000      1.000 0.000 1.000
#&gt; SRR1576849     2   0.000      1.000 0.000 1.000
#&gt; SRR1576850     2   0.000      1.000 0.000 1.000
#&gt; SRR1576851     2   0.000      1.000 0.000 1.000
#&gt; SRR1576852     2   0.000      1.000 0.000 1.000
#&gt; SRR1576853     1   0.469      0.899 0.900 0.100
#&gt; SRR1576854     1   0.494      0.891 0.892 0.108
#&gt; SRR1576855     1   0.552      0.870 0.872 0.128
#&gt; SRR1576856     1   0.563      0.865 0.868 0.132
#&gt; SRR1576857     1   0.000      0.981 1.000 0.000
#&gt; SRR1576858     1   0.000      0.981 1.000 0.000
#&gt; SRR1576859     1   0.000      0.981 1.000 0.000
#&gt; SRR1576860     1   0.000      0.981 1.000 0.000
#&gt; SRR1576861     1   0.000      0.981 1.000 0.000
#&gt; SRR1576862     1   0.000      0.981 1.000 0.000
#&gt; SRR1576863     2   0.000      1.000 0.000 1.000
#&gt; SRR1576864     2   0.000      1.000 0.000 1.000
#&gt; SRR1576865     2   0.000      1.000 0.000 1.000
#&gt; SRR1576866     2   0.000      1.000 0.000 1.000
#&gt; SRR1576867     1   0.000      0.981 1.000 0.000
#&gt; SRR1576868     1   0.000      0.981 1.000 0.000
#&gt; SRR1576869     1   0.000      0.981 1.000 0.000
#&gt; SRR1576870     1   0.000      0.981 1.000 0.000
#&gt; SRR1576871     1   0.000      0.981 1.000 0.000
#&gt; SRR1576872     1   0.000      0.981 1.000 0.000
#&gt; SRR1576873     2   0.000      1.000 0.000 1.000
#&gt; SRR1576874     2   0.000      1.000 0.000 1.000
#&gt; SRR1576875     2   0.000      1.000 0.000 1.000
#&gt; SRR1576876     2   0.000      1.000 0.000 1.000
#&gt; SRR1576877     1   0.000      0.981 1.000 0.000
#&gt; SRR1576878     1   0.000      0.981 1.000 0.000
#&gt; SRR1576879     1   0.000      0.981 1.000 0.000
#&gt; SRR1576880     2   0.000      1.000 0.000 1.000
#&gt; SRR1576881     2   0.000      1.000 0.000 1.000
#&gt; SRR1576882     2   0.000      1.000 0.000 1.000
#&gt; SRR1576883     2   0.000      1.000 0.000 1.000
#&gt; SRR1576884     2   0.000      1.000 0.000 1.000
#&gt; SRR1576885     2   0.000      1.000 0.000 1.000
#&gt; SRR1576886     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1576833     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576834     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576835     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576836     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576837     3  0.2173      0.923 0.048 0.008 0.944
#&gt; SRR1576838     3  0.1989      0.923 0.048 0.004 0.948
#&gt; SRR1576839     3  0.1774      0.935 0.016 0.024 0.960
#&gt; SRR1576840     3  0.1774      0.935 0.016 0.024 0.960
#&gt; SRR1576841     3  0.0892      0.930 0.020 0.000 0.980
#&gt; SRR1576842     3  0.0892      0.930 0.020 0.000 0.980
#&gt; SRR1576843     3  0.1289      0.921 0.032 0.000 0.968
#&gt; SRR1576844     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576845     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576846     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576847     2  0.2939      0.919 0.072 0.916 0.012
#&gt; SRR1576848     2  0.2939      0.919 0.072 0.916 0.012
#&gt; SRR1576849     2  0.3091      0.916 0.072 0.912 0.016
#&gt; SRR1576850     2  0.1163      0.952 0.028 0.972 0.000
#&gt; SRR1576851     2  0.1031      0.953 0.024 0.976 0.000
#&gt; SRR1576852     2  0.1289      0.950 0.032 0.968 0.000
#&gt; SRR1576853     3  0.3572      0.896 0.040 0.060 0.900
#&gt; SRR1576854     3  0.3764      0.887 0.040 0.068 0.892
#&gt; SRR1576855     3  0.5153      0.819 0.068 0.100 0.832
#&gt; SRR1576856     3  0.5153      0.819 0.068 0.100 0.832
#&gt; SRR1576857     3  0.0000      0.939 0.000 0.000 1.000
#&gt; SRR1576858     3  0.0000      0.939 0.000 0.000 1.000
#&gt; SRR1576859     3  0.0000      0.939 0.000 0.000 1.000
#&gt; SRR1576860     3  0.0424      0.939 0.008 0.000 0.992
#&gt; SRR1576861     3  0.0424      0.939 0.008 0.000 0.992
#&gt; SRR1576862     3  0.0424      0.939 0.008 0.000 0.992
#&gt; SRR1576863     2  0.0747      0.959 0.016 0.984 0.000
#&gt; SRR1576864     2  0.0747      0.959 0.016 0.984 0.000
#&gt; SRR1576865     2  0.0592      0.960 0.012 0.988 0.000
#&gt; SRR1576866     2  0.0592      0.960 0.012 0.988 0.000
#&gt; SRR1576867     1  0.2625      0.902 0.916 0.000 0.084
#&gt; SRR1576868     1  0.2625      0.902 0.916 0.000 0.084
#&gt; SRR1576869     1  0.2625      0.902 0.916 0.000 0.084
#&gt; SRR1576870     1  0.5497      0.754 0.708 0.000 0.292
#&gt; SRR1576871     1  0.5497      0.754 0.708 0.000 0.292
#&gt; SRR1576872     1  0.5497      0.754 0.708 0.000 0.292
#&gt; SRR1576873     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576874     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576875     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576876     2  0.0000      0.962 0.000 1.000 0.000
#&gt; SRR1576877     1  0.2625      0.902 0.916 0.000 0.084
#&gt; SRR1576878     1  0.2625      0.902 0.916 0.000 0.084
#&gt; SRR1576879     1  0.2625      0.902 0.916 0.000 0.084
#&gt; SRR1576880     2  0.4504      0.798 0.196 0.804 0.000
#&gt; SRR1576881     2  0.5016      0.749 0.240 0.760 0.000
#&gt; SRR1576882     2  0.5016      0.745 0.240 0.760 0.000
#&gt; SRR1576883     2  0.0424      0.961 0.008 0.992 0.000
#&gt; SRR1576884     2  0.0424      0.961 0.008 0.992 0.000
#&gt; SRR1576885     2  0.0424      0.961 0.008 0.992 0.000
#&gt; SRR1576886     2  0.0424      0.961 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1576833     4  0.4268    0.73493 0.004 0.232 0.004 0.760
#&gt; SRR1576834     4  0.4198    0.74149 0.004 0.224 0.004 0.768
#&gt; SRR1576835     4  0.4372    0.67768 0.000 0.268 0.004 0.728
#&gt; SRR1576836     4  0.4372    0.67768 0.000 0.268 0.004 0.728
#&gt; SRR1576837     3  0.6305    0.36479 0.060 0.424 0.516 0.000
#&gt; SRR1576838     3  0.6292    0.38248 0.060 0.416 0.524 0.000
#&gt; SRR1576839     3  0.5498    0.56278 0.028 0.312 0.656 0.004
#&gt; SRR1576840     3  0.5675    0.54706 0.028 0.320 0.644 0.008
#&gt; SRR1576841     3  0.1042    0.81196 0.000 0.020 0.972 0.008
#&gt; SRR1576842     3  0.1042    0.81196 0.000 0.020 0.972 0.008
#&gt; SRR1576843     3  0.1151    0.80947 0.000 0.024 0.968 0.008
#&gt; SRR1576844     4  0.2385    0.82363 0.028 0.052 0.000 0.920
#&gt; SRR1576845     4  0.2385    0.82363 0.028 0.052 0.000 0.920
#&gt; SRR1576846     4  0.2385    0.82363 0.028 0.052 0.000 0.920
#&gt; SRR1576847     4  0.3385    0.79372 0.012 0.048 0.056 0.884
#&gt; SRR1576848     4  0.3385    0.79372 0.012 0.048 0.056 0.884
#&gt; SRR1576849     4  0.3385    0.79372 0.012 0.048 0.056 0.884
#&gt; SRR1576850     4  0.3610    0.79455 0.060 0.020 0.044 0.876
#&gt; SRR1576851     4  0.3638    0.79621 0.056 0.024 0.044 0.876
#&gt; SRR1576852     4  0.3451    0.79938 0.052 0.020 0.044 0.884
#&gt; SRR1576853     2  0.4770    0.29943 0.012 0.700 0.288 0.000
#&gt; SRR1576854     2  0.4744    0.30875 0.012 0.704 0.284 0.000
#&gt; SRR1576855     2  0.4245    0.43476 0.020 0.784 0.196 0.000
#&gt; SRR1576856     2  0.4245    0.43476 0.020 0.784 0.196 0.000
#&gt; SRR1576857     3  0.0188    0.82271 0.004 0.000 0.996 0.000
#&gt; SRR1576858     3  0.0188    0.82271 0.004 0.000 0.996 0.000
#&gt; SRR1576859     3  0.0188    0.82271 0.004 0.000 0.996 0.000
#&gt; SRR1576860     3  0.1004    0.81885 0.024 0.004 0.972 0.000
#&gt; SRR1576861     3  0.1004    0.81885 0.024 0.004 0.972 0.000
#&gt; SRR1576862     3  0.1004    0.81885 0.024 0.004 0.972 0.000
#&gt; SRR1576863     2  0.2647    0.65756 0.000 0.880 0.000 0.120
#&gt; SRR1576864     2  0.2647    0.65756 0.000 0.880 0.000 0.120
#&gt; SRR1576865     2  0.2647    0.65756 0.000 0.880 0.000 0.120
#&gt; SRR1576866     2  0.2647    0.65756 0.000 0.880 0.000 0.120
#&gt; SRR1576867     1  0.2805    0.87996 0.888 0.012 0.100 0.000
#&gt; SRR1576868     1  0.2805    0.87996 0.888 0.012 0.100 0.000
#&gt; SRR1576869     1  0.2988    0.87735 0.876 0.012 0.112 0.000
#&gt; SRR1576870     1  0.4163    0.83409 0.792 0.020 0.188 0.000
#&gt; SRR1576871     1  0.4163    0.83409 0.792 0.020 0.188 0.000
#&gt; SRR1576872     1  0.4163    0.83409 0.792 0.020 0.188 0.000
#&gt; SRR1576873     4  0.3632    0.79740 0.008 0.156 0.004 0.832
#&gt; SRR1576874     4  0.3539    0.78871 0.000 0.176 0.004 0.820
#&gt; SRR1576875     4  0.3668    0.78288 0.000 0.188 0.004 0.808
#&gt; SRR1576876     4  0.3710    0.77952 0.000 0.192 0.004 0.804
#&gt; SRR1576877     1  0.1890    0.83014 0.936 0.000 0.008 0.056
#&gt; SRR1576878     1  0.1890    0.83014 0.936 0.000 0.008 0.056
#&gt; SRR1576879     1  0.1890    0.83014 0.936 0.000 0.008 0.056
#&gt; SRR1576880     4  0.2654    0.80081 0.108 0.004 0.000 0.888
#&gt; SRR1576881     4  0.2530    0.79783 0.112 0.000 0.000 0.888
#&gt; SRR1576882     4  0.2714    0.79660 0.112 0.004 0.000 0.884
#&gt; SRR1576883     2  0.5119    0.19490 0.000 0.556 0.004 0.440
#&gt; SRR1576884     2  0.5132    0.16933 0.000 0.548 0.004 0.448
#&gt; SRR1576885     2  0.5147    0.12733 0.000 0.536 0.004 0.460
#&gt; SRR1576886     2  0.5168   -0.00683 0.000 0.504 0.004 0.492
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1576833     2  0.0162      0.934 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576834     2  0.0162      0.934 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1576835     2  0.0162      0.937 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1576836     2  0.0162      0.937 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1576837     3  0.4972      0.795 0.012 0.116 0.772 0.040 0.060
#&gt; SRR1576838     3  0.5139      0.784 0.012 0.116 0.760 0.036 0.076
#&gt; SRR1576839     3  0.4393      0.793 0.004 0.160 0.780 0.036 0.020
#&gt; SRR1576840     3  0.4685      0.781 0.004 0.164 0.764 0.032 0.036
#&gt; SRR1576841     3  0.2569      0.864 0.000 0.000 0.892 0.068 0.040
#&gt; SRR1576842     3  0.2278      0.870 0.000 0.000 0.908 0.060 0.032
#&gt; SRR1576843     3  0.2694      0.861 0.000 0.000 0.884 0.076 0.040
#&gt; SRR1576844     4  0.4416      0.808 0.000 0.356 0.000 0.632 0.012
#&gt; SRR1576845     4  0.4402      0.813 0.000 0.352 0.000 0.636 0.012
#&gt; SRR1576846     4  0.4402      0.813 0.000 0.352 0.000 0.636 0.012
#&gt; SRR1576847     4  0.4323      0.824 0.000 0.240 0.004 0.728 0.028
#&gt; SRR1576848     4  0.4323      0.824 0.000 0.240 0.004 0.728 0.028
#&gt; SRR1576849     4  0.4295      0.823 0.000 0.236 0.004 0.732 0.028
#&gt; SRR1576850     4  0.4037      0.744 0.008 0.088 0.008 0.820 0.076
#&gt; SRR1576851     4  0.4095      0.742 0.008 0.088 0.008 0.816 0.080
#&gt; SRR1576852     4  0.4095      0.742 0.008 0.088 0.008 0.816 0.080
#&gt; SRR1576853     5  0.4810      0.809 0.008 0.056 0.164 0.016 0.756
#&gt; SRR1576854     5  0.4898      0.811 0.008 0.064 0.160 0.016 0.752
#&gt; SRR1576855     5  0.3678      0.877 0.004 0.068 0.080 0.008 0.840
#&gt; SRR1576856     5  0.3802      0.876 0.008 0.068 0.080 0.008 0.836
#&gt; SRR1576857     3  0.0162      0.896 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1576858     3  0.0000      0.896 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576859     3  0.0000      0.896 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1576860     3  0.0613      0.896 0.004 0.000 0.984 0.004 0.008
#&gt; SRR1576861     3  0.0613      0.896 0.004 0.000 0.984 0.004 0.008
#&gt; SRR1576862     3  0.0613      0.896 0.004 0.000 0.984 0.004 0.008
#&gt; SRR1576863     5  0.2446      0.890 0.000 0.056 0.000 0.044 0.900
#&gt; SRR1576864     5  0.2446      0.890 0.000 0.056 0.000 0.044 0.900
#&gt; SRR1576865     5  0.2446      0.890 0.000 0.056 0.000 0.044 0.900
#&gt; SRR1576866     5  0.2446      0.890 0.000 0.056 0.000 0.044 0.900
#&gt; SRR1576867     1  0.1369      0.954 0.956 0.000 0.028 0.008 0.008
#&gt; SRR1576868     1  0.1455      0.955 0.952 0.000 0.032 0.008 0.008
#&gt; SRR1576869     1  0.1538      0.955 0.948 0.000 0.036 0.008 0.008
#&gt; SRR1576870     1  0.2069      0.939 0.912 0.000 0.076 0.000 0.012
#&gt; SRR1576871     1  0.2006      0.941 0.916 0.000 0.072 0.000 0.012
#&gt; SRR1576872     1  0.2069      0.939 0.912 0.000 0.076 0.000 0.012
#&gt; SRR1576873     2  0.1205      0.931 0.004 0.956 0.000 0.040 0.000
#&gt; SRR1576874     2  0.1124      0.933 0.004 0.960 0.000 0.036 0.000
#&gt; SRR1576875     2  0.0671      0.939 0.004 0.980 0.000 0.016 0.000
#&gt; SRR1576876     2  0.0451      0.939 0.004 0.988 0.000 0.008 0.000
#&gt; SRR1576877     1  0.0771      0.938 0.976 0.000 0.000 0.020 0.004
#&gt; SRR1576878     1  0.0771      0.938 0.976 0.000 0.000 0.020 0.004
#&gt; SRR1576879     1  0.0771      0.938 0.976 0.000 0.000 0.020 0.004
#&gt; SRR1576880     4  0.4946      0.826 0.024 0.328 0.000 0.636 0.012
#&gt; SRR1576881     4  0.4879      0.825 0.020 0.332 0.000 0.636 0.012
#&gt; SRR1576882     4  0.4946      0.826 0.024 0.328 0.000 0.636 0.012
#&gt; SRR1576883     2  0.2592      0.911 0.000 0.892 0.000 0.052 0.056
#&gt; SRR1576884     2  0.2520      0.914 0.000 0.896 0.000 0.048 0.056
#&gt; SRR1576885     2  0.2661      0.907 0.000 0.888 0.000 0.056 0.056
#&gt; SRR1576886     2  0.2661      0.907 0.000 0.888 0.000 0.056 0.056
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1576833     2  0.1168     0.9322 0.000 0.956 0.000 0.016 0.000 0.028
#&gt; SRR1576834     2  0.1168     0.9322 0.000 0.956 0.000 0.016 0.000 0.028
#&gt; SRR1576835     2  0.1408     0.9266 0.000 0.944 0.000 0.020 0.000 0.036
#&gt; SRR1576836     2  0.1492     0.9269 0.000 0.940 0.000 0.024 0.000 0.036
#&gt; SRR1576837     5  0.7235     0.3539 0.044 0.036 0.200 0.008 0.496 0.216
#&gt; SRR1576838     5  0.7041     0.3602 0.052 0.020 0.196 0.008 0.512 0.212
#&gt; SRR1576839     5  0.7085     0.3494 0.012 0.044 0.212 0.020 0.504 0.208
#&gt; SRR1576840     5  0.7176     0.3679 0.016 0.052 0.192 0.024 0.520 0.196
#&gt; SRR1576841     4  0.7702    -0.0411 0.004 0.008 0.288 0.304 0.288 0.108
#&gt; SRR1576842     4  0.7702    -0.0436 0.004 0.008 0.284 0.304 0.292 0.108
#&gt; SRR1576843     4  0.7778    -0.0473 0.004 0.012 0.284 0.300 0.292 0.108
#&gt; SRR1576844     4  0.2815     0.7230 0.000 0.120 0.000 0.848 0.000 0.032
#&gt; SRR1576845     4  0.2815     0.7230 0.000 0.120 0.000 0.848 0.000 0.032
#&gt; SRR1576846     4  0.2771     0.7246 0.000 0.116 0.000 0.852 0.000 0.032
#&gt; SRR1576847     4  0.3618     0.7210 0.000 0.044 0.044 0.824 0.000 0.088
#&gt; SRR1576848     4  0.3616     0.7207 0.000 0.040 0.048 0.824 0.000 0.088
#&gt; SRR1576849     4  0.3601     0.7215 0.000 0.040 0.044 0.824 0.000 0.092
#&gt; SRR1576850     4  0.3674     0.7208 0.004 0.028 0.012 0.824 0.020 0.112
#&gt; SRR1576851     4  0.3721     0.7178 0.004 0.024 0.012 0.820 0.024 0.116
#&gt; SRR1576852     4  0.3641     0.7175 0.004 0.020 0.012 0.824 0.024 0.116
#&gt; SRR1576853     5  0.1398     0.5958 0.008 0.000 0.052 0.000 0.940 0.000
#&gt; SRR1576854     5  0.1398     0.5958 0.008 0.000 0.052 0.000 0.940 0.000
#&gt; SRR1576855     5  0.1801     0.6112 0.016 0.000 0.004 0.000 0.924 0.056
#&gt; SRR1576856     5  0.1914     0.6110 0.016 0.000 0.008 0.000 0.920 0.056
#&gt; SRR1576857     3  0.0603     0.9732 0.000 0.000 0.980 0.000 0.016 0.004
#&gt; SRR1576858     3  0.0914     0.9678 0.000 0.000 0.968 0.000 0.016 0.016
#&gt; SRR1576859     3  0.0717     0.9724 0.000 0.000 0.976 0.000 0.016 0.008
#&gt; SRR1576860     3  0.0725     0.9735 0.012 0.000 0.976 0.000 0.000 0.012
#&gt; SRR1576861     3  0.0725     0.9735 0.012 0.000 0.976 0.000 0.000 0.012
#&gt; SRR1576862     3  0.0622     0.9740 0.012 0.000 0.980 0.000 0.000 0.008
#&gt; SRR1576863     5  0.5006     0.5261 0.000 0.056 0.000 0.008 0.548 0.388
#&gt; SRR1576864     5  0.5006     0.5261 0.000 0.056 0.000 0.008 0.548 0.388
#&gt; SRR1576865     5  0.5064     0.5225 0.000 0.060 0.000 0.008 0.540 0.392
#&gt; SRR1576866     5  0.5064     0.5225 0.000 0.060 0.000 0.008 0.540 0.392
#&gt; SRR1576867     1  0.0603     0.9470 0.980 0.000 0.016 0.000 0.000 0.004
#&gt; SRR1576868     1  0.0692     0.9474 0.976 0.000 0.020 0.000 0.000 0.004
#&gt; SRR1576869     1  0.0806     0.9471 0.972 0.000 0.020 0.000 0.000 0.008
#&gt; SRR1576870     1  0.2307     0.9245 0.896 0.000 0.068 0.000 0.004 0.032
#&gt; SRR1576871     1  0.2173     0.9280 0.904 0.000 0.064 0.000 0.004 0.028
#&gt; SRR1576872     1  0.2173     0.9280 0.904 0.000 0.064 0.000 0.004 0.028
#&gt; SRR1576873     2  0.0547     0.9386 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; SRR1576874     2  0.0547     0.9386 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; SRR1576875     2  0.0632     0.9381 0.000 0.976 0.000 0.024 0.000 0.000
#&gt; SRR1576876     2  0.0547     0.9389 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; SRR1576877     1  0.1401     0.9316 0.948 0.004 0.000 0.020 0.000 0.028
#&gt; SRR1576878     1  0.1478     0.9305 0.944 0.004 0.000 0.020 0.000 0.032
#&gt; SRR1576879     1  0.1478     0.9305 0.944 0.004 0.000 0.020 0.000 0.032
#&gt; SRR1576880     4  0.2645     0.7402 0.012 0.084 0.000 0.880 0.004 0.020
#&gt; SRR1576881     4  0.2501     0.7387 0.012 0.072 0.000 0.888 0.000 0.028
#&gt; SRR1576882     4  0.2554     0.7395 0.012 0.088 0.000 0.880 0.000 0.020
#&gt; SRR1576883     2  0.2607     0.9036 0.000 0.888 0.000 0.028 0.028 0.056
#&gt; SRR1576884     2  0.2755     0.9005 0.000 0.880 0.000 0.028 0.036 0.056
#&gt; SRR1576885     2  0.3078     0.8863 0.000 0.860 0.000 0.028 0.048 0.064
#&gt; SRR1576886     2  0.3092     0.8887 0.000 0.860 0.000 0.036 0.040 0.064
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#>  [1] grid      parallel  stats4    stats     graphics  grDevices utils     datasets  methods  
#> [10] base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0           ComplexHeatmap_2.1.1        markdown_1.1               
#>  [4] knitr_1.26                  cola_1.3.2                  SummarizedExperiment_1.14.1
#>  [7] DelayedArray_0.10.0         BiocParallel_1.18.1         matrixStats_0.55.0         
#> [10] Biobase_2.44.0              GenomicRanges_1.36.1        GenomeInfoDb_1.20.0        
#> [13] IRanges_2.18.3              S4Vectors_0.22.1            BiocGenerics_0.30.0        
#> [16] GetoptLong_0.1.7           
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6           bit64_0.9-7            doParallel_1.0.15      RColorBrewer_1.1-2    
#>  [5] httr_1.4.1             backports_1.1.5        tools_3.6.0            R6_2.4.1              
#>  [9] DBI_1.0.0              lazyeval_0.2.2         colorspace_1.4-1       withr_2.1.2           
#> [13] tidyselect_0.2.5       gridExtra_2.3          bit_1.1-14             compiler_3.6.0        
#> [17] xml2_1.2.2             microbenchmark_1.4-7   pkgmaker_0.28          slam_0.1-46           
#> [21] scales_1.1.0           NMF_0.23.6             stringr_1.4.0          digest_0.6.23         
#> [25] XVector_0.24.0         pkgconfig_2.0.3        bibtex_0.4.2           highr_0.8             
#> [29] rlang_0.4.2            GlobalOptions_0.1.1    RSQLite_2.1.2          impute_1.58.0         
#> [33] shape_1.4.4            mclust_5.4.5           dendextend_1.12.0      dplyr_0.8.3           
#> [37] RCurl_1.95-4.12        magrittr_1.5           GenomeInfoDbData_1.2.1 Matrix_1.2-17         
#> [41] Rcpp_1.0.3             munsell_0.5.0          viridis_0.5.1          lifecycle_0.1.0       
#> [45] stringi_1.4.3          zlibbioc_1.30.0        plyr_1.8.4             blob_1.2.0            
#> [49] crayon_1.3.4           lattice_0.20-38        splines_3.6.0          annotate_1.62.0       
#> [53] circlize_0.4.9         zeallot_0.1.0          pillar_1.4.2           rjson_0.2.20          
#> [57] rngtools_1.4           reshape2_1.4.3         codetools_0.2-16       XML_3.98-1.20         
#> [61] glue_1.3.1             evaluate_0.14          vctrs_0.2.0            png_0.1-7             
#> [65] foreach_1.4.7          polyclip_1.10-0        gtable_0.3.0           purrr_0.3.3           
#> [69] clue_0.3-57            assertthat_0.2.1       ggplot2_3.2.1          xfun_0.11             
#> [73] gridBase_0.4-7         eulerr_6.0.0           xtable_1.8-4           skmeans_0.2-11        
#> [77] survival_2.44-1.1      viridisLite_0.3.0      tibble_2.1.3           iterators_1.0.12      
#> [81] memoise_1.1.0          AnnotationDbi_1.46.1   registry_0.5-1         GTF_0.0.1             
#> [85] cluster_2.1.0          brew_1.0-6
```


