# Unilur
Eric Koncina  



**Unilur** is a [rmarkdown](http://rmarkdown.rstudio.com/) template usefull to create documents for tutorials, practicals or examens. It allows the rendering of two distinct pdf/html files from a single rmarkdown file:

- The main file containing the exercises or questions **without solutions**
- The second one **including the expected answers** to the questions/exercises

These templates introduce additional possibilities such as the:

- Creation of colored boxes to highlight some markdown or _R_ content.
- An *examen* template allowing:
    - to create multiple choice questions.
    - to draw a box asking students for their name during an examen.
    - to draw dotted lines to fill in answers

# Installation

The source code is hosted on [github](https://github.com/koncina/unilur). To install the _R_ library, run the following code:


```r
devtools::install_git('https://github.com/koncina/unilur.git')
```

The library contains example templates illustrating some of the possibilities.

# The tutorial template

To use the tutorial template, adjust the `output` format in the document header as follows:

```
output:
  unilur::tutorial_html: default
  unilur::tutorial_pdf: default
  unilur::tutorial_html_solution: default
  unilur::tutorial_pdf_solution: default
```

Use the Rstudio knit button and knit to the wanted output format. Keep in mind that the output file will end with a suffix (which can be adjusted in the YAML header):

- **`_question`** for `unilur::tutorial_html` and `unilur::tutorial_pdf`
- **`_solution`** for `unilur::tutorial_html_solution` and `unilur::tutorial_pdf_solution`

### Yaml header arguments

Additional parameters of the unilur format can adjusted in the YAML front-matter. The table below summaries the different settings (default values are **bold**):

| parameter           | value                                      | description                                      |
|---------------------|--------------------------------------------|:-------------------------------------------------|
|  question_suffix    | *text* (**\_question**)                    |  suffix appended to the question pdf/html        |
|  solution_suffix    | *text* (**\_solution**)                    |  suffix appended to the solution pdf/html        |
|  credit             | `yes`/**`no`**                             |  Show a link to this homepage                    |

## Solutions

Answers or solutions are written within a [code chunk](http://rmarkdown.rstudio.com/authoring_rcodechunks.html) with the `solution = TRUE` option. The content of these chunks will be wrapped within a green box.

    ```{r, solution = TRUE}
    # Only visible and evaluated in the solution file...
    plot(cars)
    ```

You can also write markdown formatted text by using _asis_ chunks:  
    
    ```{asis, solution = TRUE}
    # This is a title

    And the text can be formatted as **bold** or *italic*

    You can also list some items:

    - item 1
    - item 2
    ```
    
<img src="http://eric.koncina.eu/pics/r/unilur/solution.jpg" style="display: block; margin: auto;" />

## Colored boxes

To highlight some content and wrap it within a colored box, specify the `box = "<color>"` and eventually `boxtitle = "<title>"` arguments in the chunk (use the colors defined by `colors()` or define your own).


    ```{asis, box = "red", boxtitle = "My red box"}
    This is a **red** box and titled *My red box*
    ```
    
    ```{asis, box = "green"}
    This is a **green** box without title
    ```

    ```{asis, box = rgb(159, 129, 247, maxColorValue = 255), boxtitle = "Custom color"}
    It is also possible to use **custom colors** defined as hexadecimal color codes or using the `rgb()` function. 
    ```

<img src="http://eric.koncina.eu/pics/r/unilur/colorbox.jpg" style="display: block; margin: auto;" />

## The examen template

The `unilur::examen_pdf` and `unilur::examen_pdf_solution` templates (relying on the latex [exam class](https://www.ctan.org/pkg/exam)) extend the possibilities of the tutorial template:

- include an **identification box** to ask for the student's name
- leave **space with dotted lines** between questions to write in the answers
- generate **multiple choice questions**

### Yaml header arguments

| argument | value                                                                           |  description                                   |
|----------|---------------------------------------------------------------------------------|:-----------------------------------------------|
|  id      | **`yes`**/`no`                                                                  |  draws a box to ask students for their name    |
|  mcq     | `oneparchoices`, `oneparchoicesalt`, `oneparcheckboxesalt`, `oneparcheckboxes`  |  theme for the multiple choice questions       |

### Questions

Questions are defined as fifth-level 5 headings (`##### question`). It is possible to leave some space with lines in the main pdf file allowing the students to write down their answers. Just set the `response.space = <s>` chunk argument (in inches) together with `solution = TRUE`.

    ---
    title: "Examen demo"
    author: "John Doe"
    date: "19 April, 2016"
    output:
      unilur::examen_pdf:
        id: yes
      unilur::examen_pdf_solution:
        id: yes
    ---

    ##### Write your answer below

    ```{asis, solution = TRUE, response.space = 1}
    This is the hidden answer generating lines in the main pdf file.
    ```

    ##### Second question

    ```{asis, solution = TRUE, response.space = 0.5}
    This would be the expected answer to the question
    ```

<img src="http://eric.koncina.eu/pics/r/unilur/exam_questions.jpg" style="display: block; margin: auto;" /><img src="http://eric.koncina.eu/pics/r/unilur/exam_solution.jpg" style="display: block; margin: auto;" />


### Multiple choice questions

Lists can be rendered as multiple choice questions. Write the list within an `asis` chunk and set the option `mcq = TRUE` like in the example below:

    ```{asis, mcq = TRUE}
    - Item 1
    - Item 2
    - Item 3
    - Item 4
    - Item 5
    ```

The output theme of MCQ can be adjusted in the YAML front-matter as shown below:

```
output:
  unilur::examen_pdf:
    mcq: <mcq theme>
```

The snapshots below show how the different `<mcq theme>` values look like:

|     mcq theme          |  result                                                            |
|:----------------------:|:------------------------------------------------------------------:|
| `oneparchoices`        | ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparchoices.jpg)       |
| `oneparchoicesalt`     | ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparchoicesalt.jpg)    |
| `oneparcheckboxes`     | ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparcheckboxes.jpg)    |
| `oneparcheckboxesalt`  | ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparcheckboxesalt.jpg) |

For the `oneparchoicesalt` and `oneparcheckboxesalt` options, it is possible to specify the number of items per line with the `mcq.n` chunk option (default is 3).
