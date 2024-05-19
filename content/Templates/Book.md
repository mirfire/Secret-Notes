<%_*
	let title = tp.file.title
	let bookName = await tp.system.prompt("Name of Book ?")
	if (title.startsWith("Untitled")) {
		await tp.file.rename(`${bookName}`);
	}
	const dv = app.plugins.plugins.dataview.api
	const principleTags = 
		Object.keys(app.metadataCache.getTags())
			.map(x => x.replace("#", ""))
			.filter(function (str) {return str.includes("Principle/");}).sort()
	
	let principleTag = await tp.system.suggester((t) => t, allTags, false, "Select the Principle/Challenge" + selectedTags.join(", "))
	var challengePrinciple = principleTag.replace("Principle/", "")
	let challengeLevel = await tp.system.prompt("Challenge level ?")
_%>
---
tags:
  - Book
  - <% principleTag %>
---

Challenge: **[[<% challengePrinciple %>]]** <% challengeLevel %>

## Contents

> ???