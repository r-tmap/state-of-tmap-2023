---
title: "Workshop tmap v4"
author: "Martijn Tennekes"
date: "2023-07-27"
output: 
  html_document:
    toc: true
    keep_md: true
    toc_float:
      collapsed: false
      smooth_scroll: false
    toc_depth: 2
link-citations: yes
vignette: >
  \usepackage[utf8]{inputenc}
  %\VignetteIndexEntry{tmap v4: a sneak peek}
  %\VignetteEngine{knitr::rmarkdown}
editor_options: 
  chunk_output_type: console
---





## Visual variables

### Constant values


```r
tm_shape(World) +
	tm_polygons(fill = "gold", 
				col = "purple", 
				lwd = 2, 
				lty = "dashed", 
				fill_alpha = 0.8, 
				col_alpha = 0.5)
```

![](tmap_workshop_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

### Visual variables


```r
tm_shape(World) +
	tm_polygons(fill = "HPI", 
				col = "inequality", 
				lwd = "economy", 
				lty = "income_grp", 
				fill_alpha = "life_exp", 
				col_alpha = "footprint")
```

![](tmap_workshop_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

### Associated arguments

Each visual variable has three arguments: `.scale`, `.legend`, and `.free`. The latter related to small multiples (facets) and will be illustrated in that part.



```r
tm_shape(World) +
	tm_polygons(
		fill = "HPI", 
		fill.scale = 
					tm_scale_continuous(values = rev(terrain.colors(10))),
		fill.legend = tm_legend(orientation = "landscape", title.align = "center")
)
```

![](tmap_workshop_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

## Facetting (one dimension)


```r
tm_shape(World) +
	tm_polygons(fill = "HPI") +
	tm_facets("continent")
```

![](tmap_workshop_files/figure-html/unnamed-chunk-5-1.png)<!-- -->



```r
tm_shape(World) +
	tm_polygons(fill = "HPI", fill.free = TRUE) +
	tm_facets("continent")
```

![](tmap_workshop_files/figure-html/unnamed-chunk-6-1.png)<!-- -->



```r
tm_shape(World) +
	tm_polygons(fill = "HPI", fill.free = TRUE) +
	tm_facets_hstack("continent")
```

![](tmap_workshop_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

## Facetting (two dimensions)


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
World = World |> 
	mutate(HPI3 = cut(HPI, c(10, 20, 30, 45)),
		   inequality3 = cut(inequality, c(0, 0.2, 0.25, 0.6))) 
tm_shape(World) +
	tm_polygons(fill = "HPI") +
	tm_facets_grid(rows = "HPI3", columns = "inequality3", drop.NA.facets = TRUE)
```

![](tmap_workshop_files/figure-html/unnamed-chunk-8-1.png)<!-- -->



```r
tm_shape(World) +
	tm_polygons(fill = "grey95") +
tm_shape(World) +
	tm_polygons(fill = "footprint") +
	tm_facets_grid(rows = "HPI3", columns = "inequality3", drop.NA.facets = TRUE, free.coords = FALSE) +
	tm_title("HPI (rows) by inequality (columns)")
```

![](tmap_workshop_files/figure-html/unnamed-chunk-9-1.png)<!-- -->





