#!/usr/bin/env python
# coding: utf-8
# emacs: -*- mode: python; py-indent-offset: 4; indent-tabs-mode: nil -*-
# vi: set ft=python sts=4 ts=4 sw=4 et:

# DTIFIT

# THIS CODE IS MEANT TO RUN FROM WITHIN THE /1/ or /2/ DIRECTORIES OR ELSE YOU WILL ENCOUNTER ERRORS

#
# Creating a script to run dtifit on all patients 
#

#
# Import Modules
#

import _utilities as util
import os
from nipype.interfaces import fsl
import shutil

#
# Create Environtment
#

# Sets working directory and calls the dti and input folders

subject_dir = os.getcwd()
###temp = subject_dir.split(os.sep)  # splits the string into a list, using the os.sep char (/ in *nix)
###if '.py' in temp[-1]:  # if the last element has '.py' in it
###   temp.pop()  # pop off the last element.
###subject_dir = os.sep.join(temp)  # rejoin the elements of the list into a string, using the os.sep character between each element
###if '.py' in subject_dir[-1]:
###   subject_dir.pop()
print('subject_dir')
print(subject_dir)
### Debugging directory check
###for root, subdir, files in os.walk(subject_dir):
###    print('root:', root)
###    print('subdir:', subdir)
###    print('files:',files)
###    break
dti_dir = os.path.abspath( os.path.join(subject_dir, 'dti'))
dti_input_dir = os.path.abspath( os.path.join(dti_dir, 'input'))
print('DTI Directory')
print(dti_dir)
print('DTI Input Directory')
print(dti_input_dir)

# Shortcuts to save time later
eddy_prefix    = 'eddy'
input_path      = dti_input_dir
output_prefix   = 'dtifit'
output_path     = '../dti/dtifit'

# Where I want the data to be stored
output_basename = os.path.abspath(os.path.join(dti_dir, output_path))
print('Output Basename')
print(output_basename)

# Where I call my bval from
infinite_path = os.path.join(os.getenv('INFINITE_PATH'), 'infinite')
print('infinite_path')
print(infinite_path)

# More shortcuts to save time and space
dti30 = 'dti30.nii.gz'
dti30_brain = 'bet.b0.dti30.nii.gz'
dti30_mask = 'bet.b0.dti30_mask.nii.gz'
### Debugging Check
###print('Quitting for debugging purposes')
###quit()

#
# DTI Correction
#

dti = fsl.DTIFit()
dti.inputs.dwi = os.path.join(input_path, eddy_prefix + '.nii.gz')
dti.inputs.bvecs = os.path.join(input_path, eddy_prefix + '.eddy_rotated_bvecs')
dti.inputs.bvals = os.path.abspath(os.path.join(infinite_path, 'dti30.bval')) 
dti.inputs.base_name = output_basename
dti.inputs.mask = os.path.join(input_path, dti30_mask)
dti.cmdline

# Creating output directory if doesn't exist
if not os.path.exists(output_basename):
	os.makedirs(output_basename)

print('DTI Command line')
print(dti.cmdline)

res = dti.run()

exit()

# Move the Data

# This Code seems to place the DTI inputs in the dti_dir instead of the output_basename
# For this reason, I created this shutil script to take the dtifit_ files and move them into the correct position
# I will make a point of fixing this in the code for the future

# NOTE: THIS IS IN A SEPARATE SCRIPT CALLED nipype_mv.py BUT IT IS HERE TO SAVE SPACE AND I INTEND TO INTEGRATE THEM

src = dti_dir
dst = output_basename
files = os.listdir(src)
print('files')
print(files)
for f in files:
    if (f.startswith("dtifit_")):
        shutil.move(os.path.abspath(os.path.join(src,f)), dst)
