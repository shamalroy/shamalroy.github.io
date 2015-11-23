---
layout: post
title: AEM - Using PageFilter to filter children of a page
date: 2015-11-23 00:00:00
---

## getPageFilter() method

```java
    public static PageFilter getPageFilter (final String filterTemplatePath, final String filterValue) {
        PageFilter pageFilter = new PageFilter() {
            public boolean includes(Page p) {
                ValueMap props = p.getProperties();
                String templatePath = props.get("cq:template", String.class);
                String customPropertyName = props.get("customPropertyName", String.class);
                return (filterTemplatePath.equals(templatePath) && customPropertyName.equalsIgnoreCase(filterValue)) ? true : false;
            }
        };
        return pageFilter;
    }
```

## Call the method

```java
    Iterator<Page> children = parentPage.listChildren(getPageFilter(template, value));
```