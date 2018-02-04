# Kisudsoe's Blog

- Here is Kim SS's blog sources.

- Here is R code examples

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

----

My blog page is powered by Jekyll & [Project Pages](https://github.com/projectpages/project-pages/wiki/).