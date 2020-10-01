# MPMP pathway records

MPMP is a manually curated pathway resource for the malaria parasite. It was created as bitmaps (GIF images) with coordinate maps marking links to other pathways, and highlighting enzymes by EC number. 



## What we demo here
The same menu format and a popup with details - in the implementation, this would be a React component.

No zoom, since there's no point to zoom into an image, and no ability to drag components.

Some elements permanently highlighted (KAHRP, PfEMP1, and STEVOR).

There's still a map and navigation as originally, except clicking on enzymes and compounds would open a popup, and clicking on other pathways would navigate to other PlasmoDB pages.

On hover, the elements light up. This is implemented by overlaying a canvas on the image, and can also be used to overlay charts.

## What the equivalent of a cytoscape drawing is expected to have
Same menus (File / Layout / Paint Enzymes)
Same functioning of the menu options - experiment selector, etc.
Same ability to zoom on the right
Same capability to open a popup and show the right data for it, when asked

Image as the background
Invisible rectangles corresponding to locations of compounds in the image - those were nodes, drawn as a circle. Clickable to open the popup with compound info.
Invisible rectangles corresponding to locations of enzymes in the image - those were edge centres or elements in groups. Clickable to open the popup with enzyme info + experiment info

Ability to highlight some enzymes by drawing frames around them.
Ability to draw pictures related to enzymes, near their labels

## How current cytoscapes are made
The cytoscape is a React component.

Here it is, it's a really long piece of code:
https://github.com/VEuPathDB/ApiCommonWebsite/blob/master/Site/webapp/wdkCustomization/js/client/components/records/PathwayRecordClasses.PathwayRecordClass.jsx


All the menus, popup content etc. are coded up. You can get data for an experiment, that comes as little pictures overlaid on the image, so nodes are added dynamically too.

It's constructed from the data from the WDK:
```
const { PathwayEdges, PathwayNodes } = props.record.tables;
```


## What the image maps contain
How boxes with pathway names link to the pathways
How boxes with pathway names link to the compounds
These we want to preserve, and still make linkable.

Boxes with enzymes, mostly annotated by EC number
These we want to be able to draw on.

The clocks
The clocks represent good places to draw. Much better than drawing over the enzymes.

### Other data in the image, but not in the map
the loop (means there is a subcellular location of this enzyme available)
Not an element in a map, but it can be scraped, because when it's in the picture, it also appears in the related q-tip

Which clock corresponds to which EC number - inferrable visually, but also because their maps are the closest to each other.


## Further work

### Extend the WDK record class to handle MPMP pathway data
Or possibly make a new WDK record type.

### Extend the React component
Probably quite difficult. Possibly the split would happen quite far up, but the menus etc. have to be shared.

### Adapt MPMP images

Hopefully not needed at all. If so, it's scikit-image territory.

It could, maybe, be possible and useful to:
- Change colors
- Remove things that are identifiable (loops, clocks) and replace them with JS things
- Find boundaries of pink rectangles, and map them to the nearest blue box
- Find what the arrows are for (attempted earlier, and not easy)
