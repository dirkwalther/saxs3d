# saxs3d

3D-structure model reconstruction of proteins based on SAXS profiles
Reconstruction of low resolution three-dimensional density maps
from one-dimensional Small Angle X-ray Scattering data
for biomolecules in solution

Dirk Walther1, Fred E. Cohen1 & Sebastian Doniach2

1) Departement of Cellular and Molecular Pharmacology
University of California, San Francisco, CA 94143
walther|cohen@cmpharm.ucsf.edu
current email address: walther@mpimp-golm.mpg.de

2) Departments of Applied Physics and Physics
Stanford University, Stanford, CA 94305
doniach@drizzle.stanford.edu

Reference: 

Reconstruction of low-resolution three-dimensional density maps from one-dimensional small-angle X-ray solution scattering data for biomolecules
D. Walther, F. E. Cohen and S. Doniach
J. Appl. Cryst. (2000). 33, 350-363
https://doi.org/10.1107/S0021889899015976

https://onlinelibrary.wiley.com/doi/abs/10.1107/S0021889899015976
Synopsis: A reconstruction algorithm is described to yield three-dimensional models from one-dimensional SAXS data of biomolecules in solution.


Instructions also provided in: index.html (included in the distribution)

Available Programs:

saxs3d  - model reconstruction from SAXS scattering profile data
xlattice  - display and superpositions of bead models and structures
pdb2xyz_saxs  - calculation of Debye scattering profiles from proteins in pdb format and conversion into xyz-format to be used in xlattice

Note: Executables pre-compiled on SGI IRIX64 (dir bin/),
Invoke programs just by their names without flags to get instructions

Test sets under dir examples/ (download)

Disclaimer/ Terms: Software is provided as is without any guarantee of any kind. Usage of saxs3d shall be properly referenced in publications.
Permission to  freely distribute and modify the software is granted provided that the orginal statement of authorship is not supressed.

uncompress: uncompress saxs3d.tar.Z
untar: tar -xvf saxs3d.tar

includes binary executables (bin/), source-code (source/), examples (examples/) under SAXS3D/

 
Saxs3D - 3D-reconstruction program from scattering profile data

Compile with: cc saxs3d.c -lm -o saxs3d, or cc saxs3d.c -lm -O3 -o saxs3d (with optimization)

usage: command <saxs_profile_file> -l <lattice spacing> -i <initial xyz-file> -rm_mod <mod>
        -out <id_string> -flood -steepest -q -max_s <s_max> -xyz <target xyz-file>
        -rw <weight> -tw <weight> -g <npoints> -rg <estimated Rg>

         -l      lattice spacing in Angstrom
         -i      start from a given xyz-structure
         -rm_mod        removal-attempt every cycle%mod==0, default:mod=1
         -out  write output to files containing id_string, default: id_string=test
                  models are continueously written to lattice_id_string.xyz
                  profiles are continueously written to saxsProfile_id_string.dat
                  profile fit-scores are continueously written to scores_id_string.dat

         -flood      keep adding beads until no improvement can be made, then remove
         -steepest  find the steepest descent move (i.e. exhaustive search, slow!)
         -q             input saxsfile is in q-scale (q=2Pi s)
         -max_s     chop off profile beyond s_max
         -xyz          fill a lattice to match a target xyz-structure (no minimization)
         -rw           weight on correlation componenet of scoring function, default weight=10.0
         -tw           weight on deviations in the tail of the profile, default weight=3.0
         -g             <npoints> points to consider in Guinier fit, default: npoints=7
         -rg            <Rg> slope of Guinier fit according to estimated Rg
         -v             verbose output to the screen

Example:  saxs3d saxs_2bb2.dat -l 12 -v -out 2bb2
                 input file saxs_2bb2.dat contained in directory examples/
                 xyz-models are written to lattice_2bb2.xyz
                 profiles are written to saxsProfile_2bb2.dat
                 scores are written to scores_2bb2.dat
NOTE: input profile has to start with the first truly measured intensity, i.e. the beam stop has to be removed.
 
 
XLATTICE - Visualization of lattice models and macromolecular structures
(Xwindow program, runs on all UNIX platforms running X)

Compile with: cc Xlattice.c -lm -lX11 -o xlattice

Usage: (also see xlattice.gif (part of the distribution) for further instructions)

Xlattice v990315, Author: Dr Dirk Walther (UCSF)
 

usage: command model1.xyz [model2.xyz]

        maximally two structures at a time, extension '.xyz' required

        Format of xyz-file:

                %i (number_of_beads)
                %f %f (bead_radius  considered_as_bonded_cutoff_length)
                %s %f %f %f (label x-coord y-coord z-coord)
                .
                .
                .

Options
        -rms   calculate overlap (in percent) only, no display
        -grid   <GridSpacing>
                    display ruler with ticks separated at GridSpacing Angstroms
        -nosup  no superposition upon invokation of program
                    if structures are too big, superposition may take a long time.
        -f      <min number of bonds>
                    apply filter by requiring that a bead
                    has at least <min number of bonds> within <considered_as_bonded_cutoff_length>,
                    purpose: filtering of superimposed models (contrast enhancement)
        -label  show labels

Example: xlattice lattice_2bb2.xyz 2bb2_ca.xyz
 

        Mouse actions:

                left: rotate
                middle: translate
                shift+above: apply to only one model if two are loaded
                right: zoom

                left+middle: adjust drawing slab width
                right+middle: shift drawing slab in z-direction

        invisible corner-buttons:

                upper left corner: exit
                lower left corner:
                        left mouse button: calculate overlap in percent
                        right mouse button: superimpose models
                lower right corner: write current model to file named xlattice_supimp.xyz


 
 
 


pdb2xyz_saxs - calculate Debye scattering profiles and convert pdb-structures into
xyz-format


Compile with: cc pdb2xyz_saxs.c -lm -o pdb2xyz_saxs

pdb2xyz_saxs (D.Walther 1999)

usage: command <pdb_file>  C  -out <out_name>  [-het]  [-ca]

        C = chainID, '_'-take all chains, 'A'-take chain A only
               chainID has to be specified, even if it is a single chain protein (then '_')
        -out  out_name = write output to files containing <out_name> as name, default is test
        -het  (optional) - include hetero atoms in both saxs profile and xyz-output
        -ca    (optional) - write CAlpha atoms only to xyz-file

NOTE: first argument has to be the protein file, second arg the chain identifier !

Example: pdb2xyz_saxs pdb1mbo.ent _ -out test -het
                 creates: test.xyz and saxs_test.dat
                 view with xlattice test.xyz

Author:  Dirk Walther,
Current affiliation and email:
MPI Molecular Plant Physiology
Golm, Germany
walther@mpimp-golm.mpg.de

 
