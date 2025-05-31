
# 🛍️ Shopify Product Reviews Widget in React

This is a plug-and-play **React component** that loads and displays **Shopify product reviews** using a third-party `ReviewsWidget` script. All you need to do is pass the product’s `SKU`, and the widget will do the rest—show reviews, ratings, questions, and more.

---

## 📦 Features

- ✅ Dynamic widget initialization
- ✅ Full support for Shopify product reviews
- ✅ Customizable via CSS variables
- ✅ Built-in error handling
- ✅ Works with React or Next.js

---

## 🔧 Requirements

- A Shopify store with a compatible reviews widget (like Yotpo, Stamped, or a custom script).
- The global `window.ReviewsWidget` function must be loaded **before** rendering this component.

---

## 📁 File: `ProductReviews.tsx`

```tsx
'use client';

import { useEffect, useRef } from 'react';

interface ReviewsWidgetProps {
    sku: string;
}

declare global {
    interface Window {
        ReviewsWidget?: any;
    }
}

const ProductReviews: React.FC<ReviewsWidgetProps> = ({ sku }) => {
    const widgetRef = useRef<HTMLDivElement>(null);

    useEffect(() => {
        if (
            typeof window !== 'undefined' &&
            typeof window.ReviewsWidget === 'function' &&
            sku &&
            widgetRef.current
        ) {
            try {
                window.ReviewsWidget('#ReviewsWidget', {
                    store: 'your-shopify-store-name',
                    widget: 'polaris',
                    options: {
                        types: 'product_review',
                        enable_sentiment_analysis: true,
                        lang: 'en',
                        per_page: 5,
                        product_review: {
                            sku,
                            hide_if_no_results: true,
                        },
                        header: {
                            enable_summary: true,
                            enable_ratings: true,
                            enable_attributes: true,
                            enable_image_gallery: true,
                            enable_percent_recommended: false,
                            enable_write_review: true,
                            enable_ask_question: true,
                            enable_sub_header: true,
                            rating_decimal_places: 2,
                            use_write_review_button: false,
                            enable_if_no_results: false,
                        },
                        reviews: {
                            enable_avatar: true,
                            enable_reviewer_name: true,
                            enable_verified_badge: true,
                            enable_images: true,
                            enable_ratings: true,
                            enable_helpful_vote: true,
                            enable_date: true,
                            enable_third_party_source: true,
                            enable_product_name: true,
                        },
                        questions: {
                            hide_if_no_results: false,
                            enable_ask_question: true,
                            enable_ask_question_button_style: false,
                            show_dates: true,
                            grouping: sku,
                        },
                    },
                    styles: {
                        '--base-font-size': '16px',
                        '--primary-button-bg-color': '#0E1311',
                        '--primary-button-text-color': '#ffffff',
                        '--common-star-color': '#0E1311',
                        '--body-text-color': '#0E1311',
                        '--heading-text-color': '#0E1311',
                    },
                });
            } catch (error) {
                console.error('Failed to initialize ReviewsWidget:', error);
            }
        }
    }, [sku]);

    const cssVariables = {
        '--base-font-size': '16px',
        '--primary-button-bg-color': '#0E1311',
        '--primary-button-text-color': '#ffffff',
        '--common-star-color': '#0E1311',
        '--body-text-color': '#0E1311',
        '--heading-text-color': '#0E1311',
    } as React.CSSProperties;

    return <div id="ReviewsWidget" ref={widgetRef} style={cssVariables} className="px-10" />;
};

export default ProductReviews;
```

> 🔁 Replace `"your-shopify-store-name"` with your actual Shopify store handle.

---

## 💡 How to Use

1. **Include the Widget Script**  
   Add this in your HTML or Next.js `_document.tsx` or layout file:

```html
<script src="https://your-widget-provider.com/widget.js"></script>
```

> Ensure the script loads **before** the component is rendered.

2. **Use the Component in Your Page**

```tsx
import ProductReviews from './components/ProductReviews';

export default function ProductPage() {
    const productSku = 'ABC123';

    return (
        <div>
            <h1>Product Name</h1>
            <ProductReviews sku={productSku} />
        </div>
    );
}
```

---

## 🎨 Custom Styling with CSS Variables

You can easily theme the widget:

```css
--base-font-size: 16px;
--primary-button-bg-color: #0E1311;
--primary-button-text-color: #ffffff;
--common-star-color: #0E1311;
--body-text-color: #0E1311;
--heading-text-color: #0E1311;
```

These variables are set inline using the `style` prop.

---

## ⚠️ Notes

- This widget works **only on the client side**.
- Make sure `window.ReviewsWidget` is available before the component mounts.
- Supports dynamic `SKU` changes through props.
