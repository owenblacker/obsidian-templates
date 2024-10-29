## Today’s Actions


### Meetings


## Tasks

```dataviewjs
const [year, month, day] = dv.current().file.name.split('-');
const today = new Date(year, month - 1, day);
const weekNumber = Math.ceil(Math.floor((today - new Date(today.getFullYear(), 0, 1)) / (24 * 60 * 60 * 1000)) / 7);
const endDate = new Date(new Date().getFullYear(), 0, (2 + (weekNumber - 1) * 7) + 7);
const yesterday = new Date(year, month - 1, day);

dv.header(1, 'Due Tasks');
dv.taskList(
	dv.pages().file.tasks
		.where(t => !t.completed)
		.where(t => !(t.due || t.scheduled) || DateTime.now().diff(dv.date(t.due || t.scheduled),'days').days >=-2)
		.sort(t => t.priority)
		.sort(t => dv.date(t.scheduled)),
	false,
);

dv.header(1, 'Tasks completed yesterday');
dv.taskList(
	dv.pages().file.tasks
		.where(t => t.completed  && !isNaN(Date(t.completion)) && t.completion.toISODate() === yesterday.toISOString().substring(0,10))
		.sort(t => t.priority),
	false,
);

dv.header(1, 'Tasks completed today');
dv.taskList(
	dv.pages().file.tasks
		.where(t => t.completed  && !isNaN(Date(t.completion)) && t.completion.toISODate() === dv.current().file.name)
		.sort(t => t.priority),
	false,
);
```

## What did I learn today?

