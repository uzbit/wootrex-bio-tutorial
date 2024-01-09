
# Homology Path Tutorial


## Intro

Welcome to Homology Path, a software suite designed to help you optimize your DNA based workflows and experiments. Depending on your experiment parameters, there are several ways to flow your data through this system. Let's start with a basic example: Oligo Design. 

## Oligo Design

### Basic Oligo Design

Assuming your sequences of interest are DNA, you can get a set of design files to synthesize those DNA sequences from single stranded oligonucleotides. Using the Oligo Design tab, click the “Choose Files'' button to select your FASTA file containing the DNA sequence(s) you wish to synthesize. You’ll notice that there are a variety of parameters to modify the design. We’ll go over those in a minute. For now, just leave all the defaults as they are. Scroll down and you will need to enter your email and the Access Token provided by NinthBio. Next click “Run Oligo Design”. When the calculation completes, an image of the design is displayed and the design data is available for download. Let’s look at the design data files. 


### Design Data Files

In the zip file you’ll find several excel files as well as an SVG and a log file. The name of the excel files is taken from the “Design Name” field on the Oligo Design page. Since we left the defaults, this should be “design01”. Open the “design01.xlsx” file. 

The “Oligo Order Form” tab in this file has tabular information about all of the oligos required to order for the sequences provided. This should be used to order oligos from your favorite oligonucleotide manufacturer, for example, IDT, Twist, or Molecular Assemblies. There is a similar tab for design primers.

The “Oligo Plate Map” tab has information to tell automated liquid handlers what plate and well to source a given oligo from and what destination wells that oligo should go into. One oligo may go into many destination wells if its recycling is high across all the sequences given. Again, there is a similar tab for design primers. 

The other tabs provide various other design information that may be useful as well.


### Oligo Design Parameters

Now that we’ve got basic oligo design input and output, we can explore some of the more powerful features of the Oligo Design tool. First note, that under the “Design Type” heading, there are several options to choose from. These are “Maximize Recycling”, “Minimize Complexity”, “Gapped Design”, and “Naive Design”. We’ll go through these differences in more detail later. For now, let's scroll down and look at the parameters that are in common for all the design types. These start underneath the “Design Name” field. You can specify minimum, maximum, and target length for each oligo. Additionally, you can specify minimum and maximum overlap lengths. These give the Oligo Design algorithms some room to work with when discovering the optimal design for a set of sequences. Additional parameters are number of wells for the order plate size “Source Plate Size” and pooling plate size “Destination Plate Size”, the how to layout the wells in the design file “Plate Layout”, the initial primer size “Target Primer Size” and the temperature for optimizing the primer melting temperature “Primer Target TM Temp” in degrees Celsius. 


#### Maximize Recycling 

When designing libraries or other sets of DNA sequences with lots of homology shared between them, maximizing the recycling of oligos where possible will save both time and money. Currently, there is only one tunable parameter specific to “Maximize Recycling” and that’s “Estimated Identity”. This value is used to estimate how similar sequences should be in order to include them in the same partition when designing oligos. Trying different values between 0.8 and 1.0 could potentially improve the overall recycling efficiency, depending on your dataset. 


#### Minimize Complexity

If you have sequences that contain difficult to synthesize regions, such as repeats, hairpins, extremely high and extremely low GC regions, you can use this design type to help minimize synthesis issues. By default complexity minimized designs are done using gapped designs. There are two “Minimize Complexity” specific parameters that allow the user additional control over this design type: “Min Z-Score Cutoff”, “Optimize Overlap TMs”, “Temperature (C)”, and "Custom Cuts File". 
* The z-score cut-off is used to determine when an oligo pairing free energy is decidedly an outlier, with lower (more negative) values filtering out more pairs.
* Oligo overlaps can be TM optimized so that they have near identical melting temperatures.
* This temperature in degrees Celsius should be consistent with the temperature of your annealing step in assembly.
* Additionally there is an option to upload a custom cuts file that allows you to specify specific cut positions for each sequence, where a cut position is defined as either the 3p end or the 5p end of an oligo. This can be useful for ensuring that a oligos can split, or combine, some known complex region. The format of the custom cuts file can be seen in the example link where each line corresponds to a sequence id in the set of fasta files. Only the sequences you wish to specify custom cuts for need to be included here.


#### Gapped Design

This design type allows for having gaps between adjacent 5p and 3p oligos. This reduces the number of DNA bases required to synthesize a given sequence by allowing the polymerase step to fill in the missing regions during extension phases in the assembly process. There are three “Gapped Design” specific parameters that allow the user additional control over this design type: “Optimize Overlap TMs”, “Temperature (C)”, and "Custom Cuts File". See above for the description of these parameters.  


#### Naive Design

This design type creates a basic oligo design with no optimizations or gaps. Oligo recycling is still performed where possible.

## Sequence Complexity

Some DNA sequences of interest may contain characteristics that will cause a naive design and assembly to fail synthesis. Common causes are repeat regions, too high or too low GC content, or hairpins for example. These often result in truncated product, concatamers, or no product at all. Here, we term these types of sequence content 'complexity', in order to describe their affect on sequence synthesis difficulty. Using the Sequence Complexity tab, you can submit multiple DNA sequences for complexity assessment. Below is an example sequence showing all types of complexity that are assessed. 

![Sequence Complexity Example](https://user-images.githubusercontent.com/2830915/207479837-17bea843-9a36-4dbd-905b-79120fe5bf03.png)

From top to bottom the types of complexity are:
* Interspersed repeat regions - shown in light green, these are regions that share homology with each other and are likely to cause mis-annealing events during oligo assembly and PCR.
* Palindromic repeat regions - shown in yellow, these are regions that could cause hairpin formations and are likely to cause oligos to become unavailable for expected annealing.
* Low and high GC regions - shown in light blue and light red respectively, these are stretches of 40bp that have at most 30% (low) or at least 70% (high) GC content. High GC regions can cause issues with oligos finding their intended overlaps.
* Free energy self pair - shown in dark red, these are regions where a designed oligo has a free energy of annealing with itself (ΔG) that lies outside the z-score cutoff for the population of other oligos with themselves. 
* Free energy disjoint pair - shown in dark green, these are regions where the two indicated designed oligos have a free energy (ΔG) of annealing with each other that lies outside the z-score cutoff for the population of all oligo pairs that are not self or intended overlap pairs.
* Free energy overlap pair - shown in dark blue, these are regions where the two indicated designed oligos have a free energy (ΔG) of annealing with each other that lies outside the z-score cutoff for the population of all intended overlapping oligo pairs. These are oligos that are intended to overlap via the design and that their free energy was sufficiently low, such that the intended anneling may not occur. This complexity type is not shown in the example above.
