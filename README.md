# evolve-changes

**To allow 4 columns rather than 3 for Text columns with images**

Change feature-columns.liquid

From:
```
{% when 4 %}
        {%- assign grid_item_width = 'medium-up--one-half' -%}
        {%- assign max_height = 530 -%}
        
```
To:
```
{% when 4 %}
        {%- assign grid_item_width = 'medium-up--one-quarter' -%}
        {%- assign max_height = 530 -%}
        
```

**To add Google fonts**

In theme.liquid

Add:

```
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@100&family=Sacramento&display=swap" rel="stylesheet">
```
