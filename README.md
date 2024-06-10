`/layout/basic/layout.html`

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    ...
</head>
<body>
    ...
    <!--@import(/layout/cart-layer.html)-->
    <script src="https://unpkg.com/htmx.org@1.9.12"></script>
</body>
</html>
```

---

`/layout/basic/header.html`
```html
<a
    href="/order/basket.html"
    module="Layout_orderBasketcount"
    hx-get="/order/cart-layer.html"
    hx-target="#cart-layer .main"
    hx-trigger="click"
>
    <!--@import(/svg/icon-cart.html)--><span class="count {$basket_count_display|display} {$basket_count_display_class}"><span class="{$basket_count_class}">{$basket_count}</span></span>
</a>
```

---

`/layout/cart-layer.html`

```html
<script>
window.onload = () => {
    const cartLayer = document.getElementById('cart-layer')

    if (!cartLayer) {
        return
    }

    const open = () => {
        cartLayer.classList.add('opening')
            setTimeout(() => {
                cartLayer.classList.add('opened')
        })
    }

    const close = () => {
        cartLayer.classList.remove('opened')
            setTimeout(() => {
                cartLayer.classList.remove('opening')
            }, 200)
    }

    document.querySelector('a[href="/order/basket.html"]')?.addEventListener('click', open)
    cartLayer.querySelector('& > .backdrop')?.addEventListener('click', close)
}
</script>

<div id="cart-layer">
    <div class="backdrop"></div>
    <div class="main"><!--@import(/order/basket-layer.html)--></div>
</div>

<style>
.xans-order-layerbasketpackage {
    display: none !important;
}

#cart-layer {
    :is(&:not(.opening), & #orderFixArea) {
        display: none;
    }

    & > .backdrop {
        content: '';
        display: block;
        position: fixed;
        z-index: 999;
        inset: 0;
        background-color: rgb(0 0 0 / 25%);
        opacity: 0;
        transition: opacity 0.2s ease-in-out;
    }

    & > .main {
        position: fixed;
        z-index: 1000;
        top: 0;
        bottom: 0;
        right: 0;
        max-width: 50vw;
        padding: 1rem;
        background-color: #fff;
        border-left: 1px solid #000;
        overflow: scroll;
        transform: translateX(100%);
        transition: transform 0.2s ease-in-out;
    }

    &.opening.opened {
        & > .backdrop {
            opacity: 1;
        }
        & > .main {
            transform: translateX(0);
        }
}
</style>
```

---

`/skin-base/product/add_basket2.html`
```html
<script>
    document.querySelector(`a[href="/order/basket.html"]`)?.dispatchEvent(new Event('click'));
</script>
```

---

`/skin-base/order/basket.html`의 `<div module="Order_BasketPackage" />` 부분만 `/skin-base/order/cart-layer.html`에 저장

```html
<div module="Order_BasketPackage" class="section">
    ...
</div>
```
