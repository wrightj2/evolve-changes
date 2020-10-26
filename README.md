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

**To hide prices and add to cart unless loggedin**

In 'product-template.liquid' find 'product__price' and wrap code with IF statement as below

```
{% if customer %}
          <div class="product__price">
            {% include 'product-price', variant: current_variant, show_vendor: section.settings.show_vendor %}
          </div>
        {% else %}
		<b>Please sign-in with your trade account to see prices</b>
		{% endif %}
```


Also find 'add_to_cart' and wrap in same if statement as below

```
{% comment %}
    NC CUSTOM CODE TO Hide Add to Cart/Buy Now unless CUSTOMER LOGGEDIN
  {% endcomment %}
{% if customer %}
            <div class="product-form__controls-group product-form__controls-group--submit">
              <div class="product-form__item product-form__item--submit
                {%- if section.settings.enable_payment_button %} product-form__item--payment-button {%- endif -%}
                {%- if product.has_only_default_variant %} product-form__item--no-variants {%- endif -%}"
              >
                <button type="submit" name="add"
                  {% unless current_variant.available %} aria-disabled="true"{% endunless %}
                  aria-label="{% unless current_variant.available %}{{ 'products.product.sold_out' | t }}{% else %}{{ 'products.product.add_to_cart' | t }}{% endunless %}"
                  class="btn product-form__cart-submit{% if section.settings.enable_payment_button %} btn--secondary-accent{% endif %}"
                  data-add-to-cart>
                  <span data-add-to-cart-text>
                    {% unless current_variant.available %}
                      {{ 'products.product.sold_out' | t }}
                    {% else %}
                      {{ 'products.product.add_to_cart' | t }}
                    {% endunless %}
                  </span>
                  <span class="hide" data-loader>
                    {% include 'icon-spinner' %}
                  </span>
                </button>
                {% if section.settings.enable_payment_button %}
                  {{ form | payment_button }}
                {% endif %}
{% endif %}
```

In product-price-listing.liquid and wrap in same if statement as below

```
{% if customer %}
<dl class="price price--listing
  {%- if available == false %} price--sold-out {% endif -%}
  {%- if compare_at_price > price %} price--on-sale {% endif -%}
  {%- if product.price_varies == false and product.compare_at_price_varies %} price--compare-price-hidden {% endif -%}
  {%- if variant.unit_price_measurement %} price--unit-available {% endif -%}"
>
  {% if show_vendor and product %}
    <div class="price__vendor price__vendor--listing">
      <dt>
        <span class="visually-hidden">{{ 'products.product.vendor' | t }}</span>
      </dt>
      <dd>
        {{ product.vendor }}
      </dd>
    </div>
  {% endif %}

  {%- comment -%}
    Explanation of description list:
      - div.price__regular: Displayed when there are no variants on sale
      - div.price__sale: Displayed when a variant is a sale
      - div.price__unit: Displayed when the first variant has a unit price
      - div.price__availability: Displayed when the product is sold out
  {%- endcomment -%}
  <div class="price__regular">
    <dt>
      <span class="visually-hidden visually-hidden--inline">{{ 'products.product.regular_price' | t }}</span>
    </dt>
    <dd>
      <span class="price-item price-item--regular">
        {%- if product.price_varies -%}
          {{ 'products.product.from_lowest_price_html' | t: lowest_price: money_price }}
        {%- else -%}
          {{ money_price }}
        {%- endif -%}
      </span>
    </dd>
  </div>
  <div class="price__sale">
    <dt>
      <span class="visually-hidden visually-hidden--inline">{{ 'products.product.sale_price' | t }}</span>
    </dt>
    <dd>
      <span class="price-item price-item--sale">
        {%- if product.price_varies -%}
          {{ 'products.product.from_lowest_price_html' | t: lowest_price: money_price }}
        {%- else -%}
          {{ money_price }}
        {%- endif -%}
      </span>
    </dd>
    <div class="price__compare">
      <dt>
        <span class="visually-hidden visually-hidden--inline">{{ 'products.product.regular_price' | t }}</span>
      </dt>
      <dd>
        <s class="price-item price-item--regular">
          {{ compare_at_price | money }}
        </s>
      </dd>
    </div>
  </div>
  <div class="price__unit">
    <dt>
      <span class="visually-hidden visually-hidden--inline">{{ 'products.product.unit_price_label' | t }}</span>
    </dt>
    <dd class="price-unit-price">
      {%- capture unit_price_separator -%}
        <span aria-hidden="true">/</span><span class="visually-hidden">{{ 'general.accessibility.unit_price_separator' | t }}&nbsp;</span>
      {%- endcapture -%}
      {%- capture unit_price_base_unit -%}
        <span>
          {%- if variant.unit_price_measurement -%}
            {%- if variant.unit_price_measurement.reference_value != 1 -%}
              {{- variant.unit_price_measurement.reference_value -}}
            {%- endif -%}
            {{ variant.unit_price_measurement.reference_unit }}
          {%- endif -%}
        </span>
      {%- endcapture -%}

      <span>{{ variant.unit_price | money }}</span>{{- unit_price_separator -}}{{- unit_price_base_unit -}}
    </dd>
  </div>
  <div class="price__badges price__badges--listing">
    <span class="price__badge price__badge--sale" aria-hidden="true">
      <span>{{ 'products.product.on_sale' | t }}</span>
    </span>
    <span class="price__badge price__badge--sold-out">
      <span>{{ 'products.product.sold_out' | t }}</span>
    </span>
  </div>
</dl>
{% endif %}
```

