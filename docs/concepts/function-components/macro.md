---
title: #[function_component]
description: The #[function_component] attribute
---


The `#[function_component]` attribute allows you automatically to generate the requirements for creating Yew function components. Any Rust function can be annotated and used as a function component. 

A function annotated with `#[function_component]` may take one argument for props of `Properties` type's reference and must return `Html`. The name of component is passed as an attribute to the said attribute. Note that unlike struct component, the `Properties` type for function components must also implement `PartialEq`.

## Example

<!--DOCUSAURUS_CODE_TABS-->
<!--With props-->
```rust
#[derive(Properties, Clone, PartialEq)]
pub struct RenderedAtProps {
    pub time: Date,
}

#[function_component(RenderedAt)]
pub fn rendered_at(props: &RenderedAtProps) -> Html {
    html! {
        <p>
            <b>{ "Rendered at: " }</b>
            { String::from(props.time.to_string()) }
        </p>
    }
}
```

<!--Without props-->
```rust
#[function_component(App)]
fn app() -> Html {
    let (counter, set_counter) = use_state(|| 0);

    let (counter_one, set_counter_one) = (counter.clone(), set_counter.clone());
    let inc_onclick = Callback::from(move |_| set_counter_one(*counter_one + 1));

    let (counter_two, set_counter_two) = (counter.clone(), set_counter);
    let dec_onclick = Callback::from(move |_| set_counter_two(*counter_two - 1));
    
    html! {<>
        <nav>
            <button onclick=inc_onclick>{ "Increment" }</button>
            <button onclick=dec_onclick>{ "Decrement" }</button>
        </nav>
        <p>
            <b>{ "Current value: " }</b>
            { counter }
        </p>
    </>}
}
```
<!--END_DOCUSAURUS_CODE_TABS-->