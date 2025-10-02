

# Forvaltning
```dataview
table file.link as Link
from ""
where contains(file.tags, "#CVE") and contains(file.tags, "#ForvaltningFiks")

```
Alt som forvaltning burde fikse
	
# Devops
```dataview
table file.link as Link
from ""
where contains(file.tags, "#CVE") and contains(file.tags, "#DevOpsFiks")

```
 
 Alt DevOps burde fikse

# NVM

```dataview
table file.link as Link
from ""
where contains(file.tags, "#CVE") and contains(file.tags, "#NVMFiks")

```
 Alt nvm må fikses


```dataviewjs
for (let page of dv.pages("")
    .where(p => p.file.tags.includes("#CVE") && p.file.tags.includes("#NVMFiks"))) {
    
    // Load the full content of the note
    let content = await dv.io.load(page.file.path);

    // Extract URLs from the content
    let urls = content.match(/https?:\/\/[^\s)]+/g) || [];

    dv.table(["File", "URLs"], [
        [page.file.link, urls.map(u => `[${u}](${u})`).join("<br>")]
    ]);
}

```


