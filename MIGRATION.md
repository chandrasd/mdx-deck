# Migration

## Updating to MDX Deck v2

With a few exceptions, decks created with v1 should be compatible with v2. The following is a list of steps to ensure your slide deck will work with v2.

### Slide separator spacing

With v2, MDX Deck now splits slides based on the Remark AST's `thematicBreak` node. This means that the `---` (`<hr>`) used for splitting slides **must** have empty newlines surrounding it due to markdown syntax.

For example, the following **will not** be split into a new slide, but instead will be treated as a Setext-style heading:

```md
Hello
---
Not another slide
```

To fix this, be sure all slides are separated with empty newlines around the `---`:

```md
Hello

---

Another slide
```

### Syntax Highlighting

In v1, `react-syntax-highligher` was bundled with the `mdx-deck` package. This not only led to larger install sizes for people who were not using syntax highlighting, but also limited the API available and prevented the ability to update the syntax highlighting package separately from MDX Deck.

To enable syntax highlighting in v2, it's recommended to create a syntax highlighting theme that consists of one custom `code` component and use [composable themes][] to enable syntax highlighting with other themes.

[composable themes]: docs/theming.md#composing-themes

### Custom Layouts

If you've built custom layout components for your deck, the `children` prop should now be a flatter array of direct child elements from the slide.

For example, if you previously parsed children in your layout component, you should make the following change:

```jsx
// v1
const children = React.Children.toArray(props.children.props.children)
```

```jsx
// updated for v2
const children = React.Children.toArray(props.children)
```

### Theming

If you've built a custom theme, some of the theming API has changed.
Please refer to the [theming docs](docs/theming.md) for more info, and note that MDX Deck now uses [Emotion][] for theming.

### Third-Party Components

Some third-party components that rely on MDX Deck internal APIs _may_ not work with v2 yet. Please be patient and give library authors time to release new versions that are compatible with v2.

If you absolutely must use one of these libraries, you can continue to use v1 for the time being.

### Library Authors

If you've built a library or component that is meant for use with MDX Deck and relied on its internal state, you can try leveraging the new `useSteps` React hook available in the `@mdx-deck/components` package.

Feel free to reach out by opening an issue for any assistance in upgrading your library to work with this library's internal APIs.

[emotion]: https://emotion.sh
