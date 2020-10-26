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


**To add extra heading capability to slider widget**

In slideshow.liquid

From:

```
{%- unless block.settings.slide_title == blank -%}
                        <li>
                          <h2 class="h1 mega-title slideshow__title{% if section.settings.text_size == 'large' %} mega-title--large{% endif %}">
                            {{ block.settings.slide_title | escape }}
                          </h2>
                        </li>
                      {%- endunless -%}
```

To:

```
{%- unless block.settings.slide_title == blank -%}
                        <li>
                          <h2 class="h1 mega-title slideshow__title{% if section.settings.text_size == 'large' %} mega-title--large{% endif %}">
                            {{ block.settings.slide_title | escape }}
                          </h2>
                        </li>
                      {% comment %}
                        NC custom slideshow code
                      {% endcomment %}
                      <li>
                          <h2 class="h1 mega-title second__slideshow__title{% if section.settings.text_size == 'large' %} mega-title--large{% endif %}">
                            {{ block.settings.second_slide_title | escape }}
                          </h2>
                        </li>
                      {%- endunless -%}
```

And in JSON area add:

```
{
          "type": "text",
          "id": "second_slide_title",
          "label": {
            "da": "Overskrift",
            "de": "Titel",
            "en": "Second Heading",
            "es": "Título",
            "fi": "Otsake",
            "fr": "En-tête",
            "hi": "शीर्षक",
            "it": "Heading",
            "ja": "見出し",
            "ko": "제목",
            "nb": "Overskrift",
            "nl": "Kop",
            "pt-BR": "Título",
            "pt-PT": "Título",
            "sv": "Rubrik",
            "th": "ส่วนหัว",
            "zh-CN": "标题",
            "zh-TW": "標題"
          },
          "default": {
            "da": "Billeddias",
            "de": "Foto-Slide",
            "en": "Image slide",
            "es": "Diapositiva de imagen",
            "fi": "Kuvadia",
            "fr": "Diapositive (image)",
            "hi": "इमेज स्लाइड",
            "it": "Slide immagine",
            "ja": "画像スライド",
            "ko": "이미지 슬라이드",
            "nb": "Lysbilde",
            "nl": "Afbeelding dia",
            "pt-BR": "Slide de imagem",
            "pt-PT": "Diapositivo de imagem",
            "sv": "Bild i bildspel",
            "th": "สไลด์รูปภาพ",
            "zh-CN": "图片幻灯片",
            "zh-TW": "圖片投影片"
          }
        },
 ```

**To Add 'Interested In' options to Contact form**

In page.contact.liquid

Add

```
<label for="ContactFormFlavor">Interested In</label>
        <select id="ContactFormFlavor" name="contact[Interest]">
          <option>B2B</option>
          <option>Competitor Reviews & Trends </option>
          <option>Flowers 2 You</option>
        </select>
```
