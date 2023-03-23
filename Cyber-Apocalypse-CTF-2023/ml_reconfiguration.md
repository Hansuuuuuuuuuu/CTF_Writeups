# ML 1: Reconfiguration
> "As Pandora set out on her quest to find the ancient alien relic, she knew that the journey would be treacherous. The desert was vast and unforgiving, and the harsh conditions would put her cyborg body to the test. Pandora started by collecting data about the temperature and humidity levels in the desert. She used a scatter plot in an Orange Workspace file to visualize this data and identified the areas where the temperature was highest and the humidity was lowest. Using this information, she reconfigured her sensors to better withstand the extreme heat and conserve water. But, a second look at the data revealed something otherwordly, it seems that the relic's presence beneath the surface has scarred the land in a very peculiar way, can you see it?"


#### Difficulty: very easy

We are given a zip file that contains two files: `analysis.ows` and `points.csv`. The `.ows` file type is something I've never encountered before, so I did some Googling to figure out how it's used. 

I eventually opened the file using `less` and it ended up being in the `xml` format. I spotted something that can be Googled, which was `Orange.widgets.visualize.owscatterplot.OWScatterPlot` which then led me to [Orange](https://github.com/biolab/orange3)

Opening the `analysis.ows` file on Orange showed me two nodes, one named `File` and another named `Scatter Plot`. I drew a line connecting both to each other and double-clicked on Scatter Plot, which gave me the flag:

`HTB{sc4tter_pl0ts_4_th3_w1n}`