<%_*
	let title = tp.file.title
	let memoryName = await tp.system.prompt("Name of Memory ?")
	var memoryNameTag = memoryName.split(" ").join("-")
	if (title.startsWith("Untitled")) {
		await tp.file.rename(`${memoryName}`);
	}
	const dv = app.plugins.plugins.dataview.api
	const allTags = Object.keys(app.metadataCache.getTags()).map(x => x.replace("#", "")).filter(function (str) {return str.includes("Principle/");}).sort()
	
	let selectMore = true
	let selectedTags = []
	while (selectMore) {
	  let choice = await tp.system.suggester((t) => t, allTags, false, "[Select Principles (ESC when finished)] - " + selectedTags.join(", "))
	  if (!choice) {
	    selectMore = false
	  } else {
	    selectedTags.push(choice)
	  }
	}
	
	principleTags = "  - " + selectedTags.join("\n  - ")
	principles = new Map()
	for(tag in selectedTags) {
		principle = selectedTags[tag].replace("Principle/", "")
		let principleLevel = await tp.system.prompt("How much " + principle + " for " + memoryName + " ?" )
		principles.set(principle, principleLevel)
	}
	console.log(principles)
	sortedPrinciples = new Map([...principles.entries()].sort((a, b) => b[1] - a[1]))
	console.log(sortedPrinciples)
	levels = ""
	for(const [principle, level] of sortedPrinciples) {
		levels = levels + "**[[" + principle + "]]** " + level + ","
	}
	levels
	console.log(levels)
_%>
---
tags:
  - Memory/<% memoryNameTag %>
<% principleTags %>
---

<% levels %>

## Obtained by

- 
