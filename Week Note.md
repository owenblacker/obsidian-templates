# Completed Tasks

```tasks
done {{title}}
```

# What did I learn?

```dataviewjs
const heading = '## What did I learn today?';
const [dateYear, dateWeek] = dv.current().file.name.split('-W');

const startDate = new Date(dateYear, 0, (2 + (dateWeek - 1) * 7));
const endDate = new Date(dateYear, 0, (2 + (dateWeek - 1) * 7) + 7);

const format = (date) => {  
  if (!(date instanceof Date)) {
    throw new Error('Invalid "date" argument. You must pass a date instance')
  }

  const year = date.getFullYear()
  const month = String(date.getMonth() + 1).padStart(2, '0')
  const day = String(date.getDate()).padStart(2, '0')

  return `${year}-${month}-${day}`
}

const pages = dv.pages('"Daily Notes"')
	.where(p => dv.date(p.file.name).ts >= dv.date(format(startDate)).ts && 
			dv.date(p.file.name).ts < dv.date(format(endDate)).ts)
	.sort(page => page.file.name, "asc");

for (const page of pages) {
	dv.header(2, page.file.name)
	const file = app.vault.getAbstractFileByPath(page.file.path);
	const createdDate = page.file.title;
	const contents = await app.vault.read(file);
	
	const headingMatcher = heading.replace(/[/\-\\^$*+?.()|[\]{}]/g, '\\$&');
    const regex = `(^|\n)${headingMatcher}\r?\n(.*?)(\n## |\n---|$)`;
    
    for (const block of contents.match(new RegExp(regex, 'isg')) || []) {
      const match = block.match(new RegExp(regex, 'is'));
      dv.paragraph(match[2]);
    }
}
```
