
```dataview
TABLE category as "Module", status as "Statut", start as "Début", end as "Fin prévue", progress as "Progression (%)"
FROM "AI"
WHERE category
SORT start asc
```


```dataview
TABLE category as "Module", status as "Statut", start as "Début", end as "Fin prévue", progress as "Progression (%)"
FROM "AI"
WHERE category
SORT start asc
```

```dataviewjs
let pages = dv.pages('"AI"');
let avg = pages.avg(p => p.progress);
dv.paragraph("📊 Progression moyenne : **" + Math.round(avg) + "%**");
```
