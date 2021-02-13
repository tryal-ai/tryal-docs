# Question Markup

```js
[
    '(a) What is 2 + 3?',
    {
        type: 'numeric',
        part: 'a',
    },
    'Below are 4 numbers',
    {
        type: 'list',
        values: ['1', '3', '5', '8']
    },
    '(b) What number is the odd one in the series?',
    {
        type: 'multichoice',
        part: 'b',
        values: ['1', '3', '5', '8']
    },
    'Here is the floorplan of a house',
    {
        type: 'image',
        data: '~~ This would be a binary data string which can be used in place of a URL ~~'
    },
    '(c) Describe one thing wrong with the floorplan',
    {
        type: 'text',
        part: 'c',
    },
    '(d) If $y = 4x + 3$, complete the table below',
    {
        type: 'table',
        part: 'd',
        rows: [
            ['x', '1', '2', '3', '4', '5'],
            ['y', { type: 'numeric' }, { type: 'numeric' }, { type: 'numeric' }, { type: 'numeric' }, { type: 'numeric' }]
        ]
    },
    '(e) Sketch the curve $y = x^2 + 3x + 2$',
    {
        type: 'graph',
        part: 'e',
    }
]
```


With the release of a new Tryal.AI Question API, Questions are now returned in a different format. Where previously a question would return a single string which included LaTeX formatting such as `\hfill` and `\n`, the new Tryal.AI Question API returns an array of `string`s and `object`s, that are designed to provide you with the flexibility to render questions in a way you see fit.

<aside class="notice">
  Note that LaTeX math strings are unaffected, so you will still need to detect and handle the dollar wrapped notation, or use Tryal UI to do this for you
</aside>

To give an example take a look at the example body

The body to the right shows the complete set of possible markup types that may be part of the question body provided

**Numeric Type**

The numeric type, despite its misleading name, indicates that an input box should be rendered for working. THis can include algebraic, numeric or other similar workings. This directly corresponds to the `Workings` type available as part of [Tryal UI](https://github.com/tryal-ai/tryal-ui#tryal-workings)

keys | description
---- | -----------
`part` | A string indicating what question part this field refers to. When workings are returned to Tryal AI for marking they should consist of the workings themselves, the question `id`, the question `seed` and if provided the `part`. If part is not provided then question probably does not consist of multiple parts, or workings can be considered to apply to all parts

**List Type**

The list type is designed to be a list of strings. The list type will never have input fields in it, and is provided as a way to indicate that a set of values should be treated as a list. It's designed to allow you to make intelligent rendering decisions about how you show multiple related values, rather than interpreting LaTeX markup that was previously used for this purpose

keys | description
---- | -----------
`values` | The values in the list that need to be rendered. Typically a list of strings

**Multichoice Type**

The multichoice type is a markup indicating that a question answer is one of the provided choices. Tryal AI uses its core technology to generate "smart" wrong choices. These are choices that are the result of incorrect calculations, e.g. 2 - 3 would consist of both 1, -1 and 5 because one has a missing sign and 5 is the result of adding them.

The multichoice type should be rendered as a set of buttons to pick a choice from. You can decide how you wish to render multiple choice questions you end. 

<aside class="notice">
  Multichoice values are not preshuffled, meaning that you will need to shuffle them yourself before render
</aside>

keys | description
---- | -----------
`values` | The choice of possible values that the answer could be, typically not shuffled meaning that answer may be in same place each time without shuffling client side
`part` | The part of the question that this multichoice relates to. If not included, usually indicates that there are no other parts to the question

**Image Type**

The Image type is a markup indicating that some kind of graphic (typically a graph, chart, or image) should be rendered for this question. The image contains a [binary string representing the image](http://w3docs.com/snippets/html/how-to-display-base64-images-in-html.html)

Eventually this will move to a CDN based delivery approach where we provide a URL to the image rather than a base64 encoded image. 

keys | description
---- | -----------
`data` | The base64 encoded string representing the image, should be embeddable in a `img` tag

**Text Type**

The Text Type is a markup indicating that some kind of text input needs to be provided. This is typically for questions such as "is there a positive or negative correlation?", where a response is expected in prose English.

Tryal AI includes a number of mechanisms for marking written prose responses for questions that require them.

keys | description
---- | -----------
`part` | The part of the question for which this input relates. If this key is not present it means the input relates to the entire question or that the question consists of only a single part

**Table Type**
The table type is a markup indicating that a table needs to be rendered. This table may either be static (i.e. only containing strings) or may itself contain inputs such as numeric inputs. This is useful in situations where students are asked to "complete the table".

A table markup is provided as a set of rows, which are themselves an array representing individual values for each cell in that row.

keys | description
---- | -----------
`rows` | An array of arrays where each array represents a row, and each value within the array represents a cell in an adjacent column starting from the left to right
`part` | A value indicating the part to which the table belongs, if the table contains input markup

**Graph Type**
The graph type is a markup indicating that a graph input needs to be rendered. This graph input should use the TryGraph element provided by Tryal UI. TryGraph was designed not only to allow drawing in browser, but to provide meaningful data for marking on Tryal.AIs API service

keys | description
---- | -----------
`part` | The part to which any sketched or drawn answers relates. If a part is not included it indicates the answer relates to the whole question, or that the question does not consist of more than one part
