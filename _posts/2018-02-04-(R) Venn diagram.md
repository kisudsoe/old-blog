---
layout:    post
title:     (R) Venn diagram
author:    Kim SS
tags: 	   post Rcode
subtitle:  R code for Venn diagram
category:  post
---

# Venn diagram

## Features

- Unlimited venn input groups
- You don'k need count numbers. Just input the whole lists!

Prepare input data as list format


```R
grouplist = list(HS=c("AAA","BBB","CCC","DDD"),
                 MM=c("AAA","BBB","CCC","EEE"),
                 SC=c("AAA","BBB","FFF"),
                 EC=c("AAA","GGG"))
summary(grouplist)
```


       Length Class  Mode     
    HS 4      -none- character
    MM 4      -none- character
    SC 3      -none- character
    EC 2      -none- character


Load limma package.


```R
library(limma)
```

    Warning message:
    "package 'limma' was built under R version 3.4.3"

Unionlist is needed.


```R
unionlist = Reduce(union, grouplist)
print(unionlist)
```

    [1] "AAA" "BBB" "CCC" "DDD" "EEE" "FFF" "GGG"
​    

Get names of the groups


```R
g.title = names(grouplist)
print(g.title)
```

    [1] "HS" "MM" "SC" "EC"
​    

Togethering the vectors to one dataframe list


```R
unionPr=NULL; title=NULL
for(i in 1:length(grouplist)) {
    unionPr = cbind(unionPr,grouplist[[i]][match(unionlist,grouplist[[i]])])
    title = c(title,paste0(g.title[i],'\n',length(grouplist[[i]])))
}
rownames(unionPr) = unionlist
unionPr
```

```
    [,1]  [,2]  [,3]  [,4] 
AAA "AAA" "AAA" "AAA" "AAA"
BBB "BBB" "BBB" "BBB" NA   
CCC "CCC" "CCC" NA    NA   
DDD "DDD" NA    NA    NA   
EEE NA    "EEE" NA    NA   
FFF NA    NA    "FFF" NA   
GGG NA    NA    NA    "GGG"
```



Make TRUE/FALSE table


```R
union = (unionPr != "")     # Transform values to TRUE, if ID exists
union[is.na(union)] = FALSE # Transform NA to FALSE
union = as.data.frame(union)
colnames(union) = title
out = list(list=union, vennCounts=vennCounts(union))
out
```


    $list
        HS \n 4 MM \n 4 SC \n 3 EC \n 2
    AAA    TRUE    TRUE    TRUE    TRUE
    BBB    TRUE    TRUE    TRUE   FALSE
    CCC    TRUE    TRUE   FALSE   FALSE
    DDD    TRUE   FALSE   FALSE   FALSE
    EEE   FALSE    TRUE   FALSE   FALSE
    FFF   FALSE   FALSE    TRUE   FALSE
    GGG   FALSE   FALSE   FALSE    TRUE
    
    $vennCounts
       HS \n 4 MM \n 4 SC \n 3 EC \n 2 Counts
    1        0       0       0       0      0
    2        0       0       0       1      1
    3        0       0       1       0      1
    4        0       0       1       1      0
    5        0       1       0       0      1
    6        0       1       0       1      0
    7        0       1       1       0      0
    8        0       1       1       1      0
    9        1       0       0       0      1
    10       1       0       0       1      0
    11       1       0       1       0      0
    12       1       0       1       1      0
    13       1       1       0       0      1
    14       1       1       0       1      0
    15       1       1       1       0      1
    16       1       1       1       1      1
    attr(,"class")
    [1] "VennCounts"



Generate Venn Diagram


```R
main = "Orthologous genes in HS, MM, SC and EC"
vennDiagram(union,main=paste0(main," (",nrow(union)," genes)"), circle.col=rainbow(length(g.title)))
```


![png](/img/2018-02-04-(R)_Venn_diagram/output_14_0.png)


Here is the ss.venn function codes


```R
ss.venn = function(grouplist,main="",file=F) { # 170704 ver
	library(limma)
	# Generate union group from input three groups
	unionlist = Reduce(union, grouplist)

	# Get names from input vectors
	g.title = names(grouplist)
	
	# Togethering the vectors to one dataFrame list
	unionPr=NULL; title=NULL
	for(i in 1:length(grouplist)) {
		unionPr = cbind(unionPr,grouplist[[i]][match(unionlist,grouplist[[i]])])
		title = c(title,paste(g.title[i],'\n',length(grouplist[[i]])))
	}
	rownames(unionPr) = unionlist

	# Make TRUE/FALSE table to match with ID list
	union = (unionPr != "") 			# Transform values to TRUE, if ID exists
	union[is.na(union)] = FALSE			# Transform NA to FALSE value
	union = as.data.frame(union) 		# Make 'union' to data.frame form
	colnames(union) = title 			# Names attach to venn diagram
	out = list(list=union, vennCounts=vennCounts(union))

	## Generate Venn Diagram
	v = vennDiagram(union,main=paste0(main," (",nrow(union)," genes)"),circle.col=rainbow(length(g.title)))
	print(v)
    if(file==T) { # save image as png file
        dev.copy(png,"ss.venn.png",width=8,height=8,units="in",res=100)
        graphics.off()
    }
	return(out)
}

out = ss.venn(grouplist,main)   
```


![png](/img/2018-02-04-(R)_Venn_diagram/output_16_1.png)

