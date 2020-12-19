
To create .html from plotly:

```
from plotly.offline import plot

fig = your_plotly_figure_object
html = plot(fig, auto_open=False, output_type='div')
with open("~/ada/jczestochowska.github.io/_includes/YOUR_PLOT_NAME.html", 'w') as file:
    file.write(html)
```

To add your interactive (e.g plotly) plot:

1. Place you .html file under **_includes**
2. Include it in index.md using this syntax: 
	{% include MY_AWESOME_FILENAME.html %}
3. Push
4. Wait a **minute** and check if your plot shows correctly :) here: https://myriam23.github.io/


To add your plot as an image file:

1. Place your image in the root directory of this repository
2. Update index.md with following entry `![Title](filename.png)`
