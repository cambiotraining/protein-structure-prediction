# Publication-quality visualisations

The [ChimeraX gallery](https://www.cgl.ucsf.edu/chimerax/gallery.html) contains several examples of publication-quality visualisations, showcasing some of the advanced functionality of this software. 
Click on the `.cxc` file links to see the commands corresponding to each visualisation. 

## Surface representation

Consider the following view of the oestrogen receptor in complex with a conserved peptide motif characteristic of activator proteins (PDB: 4J26).

![](images/chimerax_4j26_surface.png)

This view combines several features that make it both visually appealing and informative. 
Here is the full list of commands to create this view, followed by a detailed explanation of each step:

```bash
close
open 4J26
delete /A,I
set bgcolor white
label delete
light simple
graphics silhouettes true width 1
show /J atoms
color /B steelblue
color /J goldenrod
color :EST red
surface /B transparency 85
surface /J transparency 85
```

- We start by closing any open structures, and opening the structure of interest.
- We have 4 chains in this structure, as two complexes were co-crystallised in the same structure. We delete one of the complexes (chains A and I) to simplify our visualisation.
- We set the background colour to white, and delete the default labels.
- We set the lighting to simple, and enable silhouettes to make the structure stand out more.
- We use `graphics silhouettes` to add a black outline to the structure, which helps to make it stand out against the white background.
- We show the atoms of chain J, which contains the coactivator peptide motif, and colour the two chains differently to make them easier to distinguish.
- We colour the ligand (EST) red to make it stand out.
- Finally, we show the molecular surface of the two chains, and set the transparency to 85 to allow us to see the secondary structure underneath the surface.

## Creating a movie

There are a few basic steps to create a movie: 

- Decide on which type of movement you would like:
  - `rock` for a back-and-forth movement around a defined axis (y-axis, i.e. vertical, by default)
  - `roll` to rotate the model 360 degrees around a defined axis (y-axis, i.e. vertical, by default)
  - `wobble` to rotate the model in a figure of eight motion
  - `turn` for a more flexible rotation definition (which can recreate `roll`, `rock` and `wobble`)
- Record the movie
- Encode the movie as a video file (e.g. mp4)

Let's see an example of how to create a rocking movie from our oestrogen receptor example. 
Using the rock command will start our rocking motion:

```bash
rock
```

To stop the motion, use the `stop` command:

```bash
stop
```

To record a movie, we can use the `movie record` command. 
However, it is useful to specify how long we want the movie to be in framerates (number of frames per second). 
This is so we end up with a movie that can be looped smoothly.

Looking at the documentation for the `rock` command, we can see the default number of frames per cycle is 136.
Therefore, we can use the `wait` command before `stop` to ensure that we record a full cycle of the rocking motion:

```bash
movie record
rock
wait 136
stop
```

Finally, we can use the `movie encode` command to save our movie as an mp4 file:

```bash
movie encode 4j26_rock.mp4 framerate 60
```

![](images/4j26_rock.gif)


<!--
NOTE: leaving this here as a note

Converting a MP4 to GIF with ffmpeg:

```bash
# this gives higher quality but larger file size
ffmpeg \
  -i 4j26_rock.mp4 \
  -r 20 \
  -vf "crop=iw*0.6:ih*0.95,scale=512:-1,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" \
  -ss 00:00:00 \
  4j26_rock.gif

ffmpeg \
  -i 4j26_rock.mp4 \
  -r 20 \
  -vf "scale=512:-1,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" \
  -ss 00:00:00 \
  4j26_rock.gif


ffmpeg \
  -i 4j26_roll.mp4 \
  -r 15 \
  -vf "crop=iw*0.5:ih*0.90,scale=512:-1" \
  -ss 00:00:00 \
  4j26_roll.gif
  
ffmpeg \
  -i 4j26_rock.mp4 \
  -r 15 \
  -vf "crop=iw*0.6:ih*0.95,scale=512:-1" \
  -ss 00:00:00 \
  4j26_rock.gif

ffmpeg \
  -i 4j26_views.mp4 \
  -r 15 \
  -vf "crop=iw*0.6:ih*0.95,scale=512:-1" \
  -ss 00:00:00 \
  4j26_views.gif
  
ffmpeg \
  -i 4j26_views_rock.mp4 \
  -r 15 \
  -vf "crop=iw*0.6:ih*0.95,scale=512:-1" \
  -ss 00:00:00 \
  4j26_views_rock.gif
``` 
-->

## Animated views

ChimeraX also allows you to create animated views, where you can define a sequence of views that are then played in a sequence.

First, let's define and name a few views of interest: 

- View 1: The entire protein
- View 2: The coactivator peptide motif
- View 3: The ligand

```bash
view protein
view name 1
view /J
view name 2
view ligand
view name 3
```

Now, we will make a sequence of these views, to record as a movie. 
The `view <name of view> <duration in framerates>` command allows us to specify how long we want each view to be shown for.
We combine this with the `wait` command, to specify how long we want the transition between views to be. 
Deciding on the framerates for each view and transition can require some trial and error, but it helps to think about it in relation to the framerate used to encode the movie at the end.

We will use 30 frames per second for our movie, so we can use this to guide our decisions on how long to show each view for, and how long to transition between views for.
For example, if we want to show a view for 2 seconds, we would specify 60 frames (30 frames/second * 2 seconds = 60 frames).

```bash
view 1
movie record
view 2 60
wait 80
view 1 30
wait 30
view 3 60
wait 80
view 1 30
wait 30
movie encode 4j26_views.mp4 framerate 30
```

- Our movie starts with view 1, which shows the entire protein structure.
- We then transition to view 2, which zooms in on the coactivator peptide motif, and show this view for 60 framerates (2 seconds).
  - Note that we wait for 80 frames before transitioning to the next view, to allow for a brief pause.
- We then zoom back out to view 1 slightly faster, showing it for 30 framerates (1 second) and immediately transition to the next view (we wait for the same number of frames as the view transition).
- Next, we transition to view 3, which zooms in on the ligand, and show this view for 60 framerates (2 seconds), pausing again for 80 frames.
- We finish by zooming back out to view 1 again, showing it for 30 framerates (1 second), before encoding the movie as an mp4 file.

![](images/4j26_views.gif)

## UniProt annotations

You can add additional features to the models, such as mutations, transmembrane regions, etc., by opening the corresponding files from UniProt. 
From the PDB entry for 4J26, we can find the UniProt ID for the oestrogen receptor is P03372.
We can open the UniProt metadata in ChimeraX using the following command:

```bash
open Q92731 from uniprot format uniprot
```

This opens a window with the sequence and annotated features. 
You can click on the features to select the corresponding residues in the structure.

For example, we can see there is a sequence variant (mutation) annotated in UniProt as: "K → R completely inactive in positive regulation of DNA-binding transcription factor activity".
This might be a mutation of interest, which we could highlight in our structure. 
By clicking on the mutation, the corresponding residues are automatically selected in the structure. 
The log window shows us that this corresponds to residue 303 in chain A, which is a lysine (K) in the wild-type structure.

We could colour this residue in the structure differently, if we wanted to make it stand out:

```bash
select /B:314
show sel atoms
style sel ball
colour sel green
select clear
```

![](images/chimerax_4j26_mutation.png)

## Tiled views

TODO: the `tile` command

## Exercises

:::{.callout-exercise}
#### Roll animation

Create a movie of the oestrogen receptor structure rotating 360 degrees around the y-axis.
Save it as a mp4 file.

![](images/4j26_roll.gif)

:::{.callout-hint}
- You can use the `roll` command to rotate the structure around the y-axis.
- Use the `wait` command to specify how long you want the movie to be in framerates (number of frames per second). From the help for the `roll` command, we can see that by default the frames rotate at 1 degree per frame, so a full 360 degree rotation would take 360 frames.
- Use the `movie record` and `movie encode` commands to save the movie as an mp4 file.
:::

:::{.callout-answer}

Here are the commands to create the movie:

```bash
roll
movie record
wait 360
stop
movie encode 4j26_roll.mp4 framerate 60
```

In this case, we have decided to use a framerate of 60 frames per second, so the movie will be 6 seconds long (360 frames / 60 frames per second = 6 seconds).
If you wanted a slower-rotating movie, you could use a lower framerate (e.g. 30 frames per second), which would make the movie 12 seconds long (360 frames / 30 frames per second = 12 seconds).

:::
:::

:::{.callout-exercise}
#### Combining animated views and rocking

Going back to our video transitioning between different views:

```bash
view 1
movie record
view 2 60
wait 80
view 1 30
wait 30
view 3 60
wait 80
view 1 30
wait 30
movie encode 4j26_views.mp4 framerate 30
```

Can you modify the code above, to include a rocking movement after zooming in on view 2 and view 3?

![](images/4j26_views_rock.gif)

:::{.callout-answer}

To achieve this, we add a `rock` command after view 2 and view 3, ensuring to include a `wait` command before stopping the motion. 
The default `rock` framerate is 136, so we set `wait` to that same number of frames. 
We therefore include the following code after view 2 and view 3:

```bash
rock
wait 136
stop
```

Here is the commented code in full:

```bash
# Start the movie with view 1
view 1
movie record

# View 2 with rocking motion
view 2 60
wait 60

# Add rocking motion to view 2
rock
wait 136
stop

# Transition back to view 1
view 1 30
wait 30

# View 3 with rocking motion
view 3 60
wait 60

# Add rocking motion to view 3
rock
wait 136
stop

# Transition back to view 1
view 1 30
wait 30

# Encode the movie as an mp4 file
movie encode 4j26_views_rock.mp4 framerate 30
```

:::
:::


:::{.callout-exercise}
#### Tiled views

Try to recreate this view of the Oestrogen Receptor complexed with different ligands (PDBs: 1GWR, 5W9C, 4XI3, and 7R62):

![Figure 1 from [Hancock et al. 2022](https://www.nature.com/articles/s41523-022-00497-9/figures/1). Figure caption from the original publication: "Ligands are shown as spheres with carbon in green, oxygen in red, and nitrogen in blue, helix 12 is colored red, the helix 11–12 loop is colored orange, and coregulator peptide in cyan."](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41523-022-00497-9/MediaObjects/41523_2022_497_Fig1_HTML.png?as=webp)

- Import all the structures into a new ChimeraX session. 
- These models each have multiple chains, but we will work with only **chain A** in each structure, plus their respective **ligands**, listed below. 
  **Create a selection** to only show these chains and ligands, and **delete the rest of the structure**.
  - `1GWR`: bound to the hormone estradiol (residues named `:EST`); also include the coregulator peptide motif (chain `/C`)
  - `5W9C`: bound to the tamoxifen-derived drug 4-hydroxytamoxifen (residues named `:OHT`)
  - `4XI3`: bound to the drug bazedoxifene (residues named `:29S`)
  - `7R62`: bound to a synthetic estradiol-like inhibitor (residues named `:3YJ`)
- Align the structures to each other to ensure similar orientations, and tile the views in a 1x4 grid.
- Colour the protein in silver, and the ligands by element (carbon in green, oxygen in red, and nitrogen in blue).
- Show the coregulator peptide motif in cyan.
- Use the sequence viewer to identify the positions of helix 12 in each structure, and colour it red, and the helix 11-12 loop in orange.

:::{.callout-answer}

We start by opening the structures of interest:

```bash
close
open 1GWR
open 5W9C
open 4XI3
open 7R62
```

We then select chain A and the ligand for each structure, and delete the rest of the structure:

```bash
select #1/A | #1/C | #1/A:EST | #2/A | #2/A:OHT | #3/A | #3/A:29S | #4/A | #4/A:3YJ
delete ~sel
select clear
```

We have opted to use the OR `|` operator to select multiple chains and ligands at the same time, but you could also do this in multiple steps if you prefer using the `select add` command. 

We then align the structures to each other to ensure similar orientations:

```bash
mm #2-4 to #1
```

We tile the views using the `tile` command, and colour the structures using `silver`, similar to the original publication:

```bash
tile column 4 spacingfactor 0.8
colour protein silver
hide protein atoms
```

We represent the ligands as spheres, and colour them by element:

```bash
show ligand atoms
style ligand sphere
colour ligand & C green
colour ligand & O red
colour ligand & N blue
```

We show the coregulator peptide in cyan:

```bash
hide #1/C atoms
cartoon #1/C
colour #1/C cyan
```

To identify the helix positions, we can use the sequence viewer, which highlights the secondary structure of each residue (helices shown as yellow boxes).
Helix 12 is the last helix of each structure, and these are its positions: 

```bash
colour #1/A:537-549 firebrick
colour #1/A:532-536 darkorange
colour #2/A:536-545 firebrick
colour #2/A:529-535 darkorange
colour #3/A:536-545 firebrick
colour #3/A:529-535 darkorange
colour #4/A:536-545 firebrick
colour #4/A:528-535 darkorange
```

Optionally, we can hide the missing residue labels, which are stored as sub-models (see `info models`):

```bash
hide #1.1.1 #2.1.1 #3.1.1 #4.1.1
```

And this gets us close to the original publication: 

![](images/chimerax_tiled_er.png)

:::
:::

:::{.callout-exercise}
#### Surface representation

Try and recreate this RCSB-PDB molecule of month view (PDB: 1AOI):

![Complex between the _Xenopus_' nucleosome (purple) with DNA (orange) wrapped around it. Image source: [RCSB PDB Molecule of the Month: Nucleosome](https://pdb101.rcsb.org/motm/7)](https://cdn.rcsb.org/pdb101/motm/7/1aoi.gif)

Make sure to colour each DNA chain slightly differently. 
You can use `log chains` to find out which chains correspond to the protein and which are the DNA.

:::{.callout-answer}

This gets us close to the visualisation:

```bash
close
open 1AOI
surface protein
surface nucleic
colour protein plum
colour /I tomato
colour /J coral
```

We used `log chains` to find out that chains I and J correspond to the DNA, and coloured them differently using the `colour` command.

:::
:::


:::{.callout-exercise}
#### UniProt annotations

Try to create a similar view to the following RBSB-PDB molecule of the month (PDB: 1SU4).

![ATP-driven calcium pump in the sarcoplasmic reticulum membrane that restores low cytosolic calcium to enable muscle relaxation. Image source: [RCSB PDB Molecule of the Month: Calcium Pump](https://pdb101.rcsb.org/motm/51)](https://cdn.rcsb.org/pdb101/motm/51/1eul-membrane.gif)

It might be hard to recreate exactly the same view, but here are some guidelines for what you could try: 

- Hide the atoms
- Draw a surface representation of the protein using a transparency of 60%
- Show the calcium ion and colour it cyan
  - Use `log metadata` to see how the Calcium molecule is called
- Import metadata from UniProt (you might have to go to PDB to find the UniProt ID). This should allow you to select: 
  - Active site: Show as atoms and colour red 
  - Topological domain lumenal: Colour `#1b9e77`
  - topological domain cytoplasmic: Colour `#7570b3`
  - transmembrane region: Colour `#d95f02`

:::{.callout-answer}

We start by opening the structure and hiding the atoms, then showing the surface representation of the protein with a transparency of 60%, and showing the CA atoms coloured cyan:

```bash
close
open 1SU4
hide cartoon
hide atoms
surface protein transparency 60
show :CA
colour :CA cyan
```

We import metadata from UniProt, using the UniProt ID P04191, which we can find in the PDB entry for 1SU4:

```bash
open P04191 from uniprot format uniprot
```

We select each of the features indicated, and colour them accordingly. 
Here, we give the exact residues, which we copied from the log window, as we clicked on each of the UniProt annotations.

```bash
select /A:70-89,274-295,778-787,852-897,950-964
colour sel #1b9e77
select /A:1-48,111-253,314-757,809-828,918-930,986-994
colour sel #7570b3
select /A:49-69,90-110,254-273,296-313,758-777,788-808,829-851,898-917,931-949,965-985
colour sel #d95f02
select /A:351
show sel atoms
style sel sphere
colour sel red
select clear
```

This should give you the following view:

![](images/chimerax_1su4.png)

:::
:::
