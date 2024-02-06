---
title: Guia de formato
layout: default
nav_order: 3
---
## Headings


# Heading 1
## Heading 2
### Heading 3

```markdown
# Heading 1
## Heading 2
### Heading 3
```



## Buttons

[Link button](https://just-the-docs.com){: .btn }
[Link button](https://just-the-docs.com){: .btn .btn-purple }


```markdown
[Link button](https://just-the-docs.com){: .btn }
[Link button](https://just-the-docs.com){: .btn .btn-purple }
```

## Tables

| head1        | head two          | three |
|-------------:|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |


```markdown
| head1        | head two          | three |
|-------------:|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |
```

## Code


```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```


## Notas

{: .highlight }
A paragraph

{: .note }
A paragraph

{: .note-title }
> My note title
> 
> A paragraph with a custom title callout


```markdown
{: .highlight }
A paragraph

{: .note }
A paragraph

{: .note-title }
> My note title
> 
> A paragraph with a custom title callout

```

## Ordering and navigation
```markdown
---
layout: default
title: Customization
nav_order: 1
---
```

Excluding pages
```markdown
nav_exclude: true
```

Parent pages:

```markdown
---
layout: default
title: UI Components
nav_order: 2
has_children: true
---
```

Child pages
```markdown
---
layout: default
title: Buttons
parent: UI Components
nav_order: 2
---
```


```markdown
```

```markdown
```

```markdown
```

```markdown
```

## Images

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png" style="width: 50px">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png" style="width: 100px">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png" style="width: 250px">
![style="width: 25px"](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png)


```markdown
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png" style="width: 50px">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png" style="width: 100px">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png" style="width: 250px">
![style="width: 25px"](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/Microsoft_Azure_Logo.svg/2560px-Microsoft_Azure_Logo.svg.png)
```


## More

- [Colors](https://just-the-docs.com/docs/utilities/color/)
- [Fonts](https://just-the-docs.com/docs/utilities/typography/)
- [Ordering](https://just-the-docs.com/docs/navigation-structure/)

- [Search configuration .yml](https://just-the-docs.com/docs/search/)
- [Practical example to implement collections](https://pdmosses.github.io/just-the-docs/)
