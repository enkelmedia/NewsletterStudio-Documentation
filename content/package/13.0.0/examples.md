---
title: How-To Guides
description: 
---


# This is version 3.0.0

This file is a template with common "snippets" to use in the docs.

Testing, mark: ==Testar== and **strong**

A link to [another doc-page](../getting-started/basics/)

## Some lists
Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Suspendisse ornare lacinia mauris ac convallis. Nullam blandit ullamcorper turpis id maximus.

A ul-list:
* Foo
* Bar
* Kalle

Lära sig: [mera info](getting-started/bar.md)

A checkbox list
* [ ] Item 1
* [ ] Item 2
* [ ] Item 3


## Code examples

### Html Example

```html
<html>
    <title>Newsletter Studio</title>
    <div class="ns-box">
        <p>Content goes here</p>
    </div>
</html>
```

### Javascript/Json

```javascript
{
    "foo" : "abv 123",
    "halloWorld" : "mackan"
}
```

### C-sharp

```csharp
public class Foo {
    public string Name {get;set;}

    public void Foo(){
        int[] nummer = new int[10];
        int[] input = new int[10];
        Console.WriteLine("Please enter 10 numbers: ");

        for (int i = 0; i < nummer.Length; i++)
        {
            Console.Write("Number {0}: ", i + 1);
            nummer[i] = int.Parse(Console.ReadLine());
            input = nummer;
        }
    }    
}

```

## Images
![alt text](/media/ss-recipients.png)


## A table

| Browser          | Deletes cookie from iframe |
|------------------|----------------------------|
| Chrome (desktop) | Yes                        |
| Chrome (mobile)  | TODO                       |
| Firefox          | Yes                        |
| IE11             | No*                        |
| Egde             | Yes                        |
| Safari (dekstop) | TODO                       |
| Safari (mobile)  | TODO                       |