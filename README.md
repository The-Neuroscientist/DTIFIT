#!/usr/bin/env python
# coding: utf-8
# emacs: -*- mode: python; py-indent-offset: 4; indent-tabs-mode: nil -*-
# vi: set ft=python sts=4 ts=4 sw=4 et:

# DTIFIT

#
# Creating a script to run dtifit on all patients 
#

#
# Import Modules
#

import _utilities as util
import os
from nipype.interfaces import fsl

#
# Create Environtment
#

# Sets working directory and calls the dti and input folders

subject_dir = os.getcwd()
temp = subject_dir.split(os.sep)  # splits the string into a list, using the os.sep char (/ in *nix)
if '.py' in temp[-1]:  # if the last element has '.py' in it
   temp.pop()  # pop off the last element.
subject_dir = os.sep.join(temp)  # rejoin the elements of the list into a string, using the os.sep character between each element
if '.py' in subject_dir[-1]:
   subject_dir.pop()
print(subject_dir)
# Debugging directory check
for root, subdir, files in os.walk(subject_dir):
    print('root:', root)
    print('subdir:', subdir)
    print('files:',files)
    break
dti_dir = os.path.abspath( os.path.join(subject_dir, 'dti'))
dti_input_dir = os.path.abspath( os.path.join(dti_dir, 'input'))
dti_output_dir = os.path.abspath( os.path.join(dti_dir, 'dtifit'))
print(dti_dir)
print(dti_input_dir)
print(dti_output_dir)
# Shortcuts to save time later
eddy_prefix    = 'eddy'
input_path      = dti_input_dir
output_prefix   = 'dtifit'
output_path     = dti_output_dir

# Where I want the data to be stored
output_basename = os.path.abspath(os.path.join(dti_dir, output_path, output_prefix))

# Where I call my bval from
infinite_path = os.path.join(os.getenv('INFINITE_PATH'), 'infinite')

# More shortcuts to save time and space
dti30 = 'dti30.nii.gz'
dti30_b0_brain = 'bet.b0.dti30.nii.gz'
dti30_b0_brain_mask = 'bet.b0.dti30_mask.nii.gz'
# Debugging Check
#print('Quitting for debugging purposes')
#quit()

#
# DTI Correction
#

dti = fsl.DTIFit()
#dti.inputs.dwi = os.path.abspath(os.path.join(input_path, eddy_prefix + '.nii.gz'))
dti.inputs.dwi = os.path.join(input_path, eddy_prefix + '.nii.gz')
# Full file-path for reference dti.inputs.dwi = '/gandg/infinite/imaging_data/individuals/inf0117/1/dti/input/eddy.nii.gz'
dti.inputs.bvecs = os.path.join(input_path, eddy_prefix + '.eddy_rotated_bvecs')
# Full file-path for reference dti.inputs.bvecs = '/gandg/infinite/imaging_data/individuals/inf0117/1/dti/input/eddy.eddy_rotated_bvecs'
dti.inputs.bvals = os.path.abspath(os.path.join(infinite_path, 'dti30.bval')) 
dti.inputs.base_name = output_path
dti.inputs.mask = os.path.join(input_path, dti30_b0_brain_mask)
# Full file-path for reference dti.inputs.mask = '/gandg/infinite/imaging_data/individuals/inf0117/1/dti/input/bet.b0.dti30_mask.nii.gz'
dti.cmdline
