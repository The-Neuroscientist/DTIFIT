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
dti_dir = os.path.abspath( os.path.join(subject_dir, 'dti'))
dti_input_dir = os.path.abspath( os.path.join(dti_dir, 'input'))

# Shortcuts to save time later
eddy_prefix    = 'eddy'
input_path      = '../dti/input'
output_prefix   = 'dtifit'
output_path     = '../dti/dti'

# Where I want the data to be stored
output_basename = os.path.abspath(os.path.join(dti_dir, output_path, output_prefix))

# Where I call my bval from
infinite_path = os.path.join(os.getenv('INFINITE_PATH'), 'infinite')

# More shortcuts to save time and space
dti30 = 'dti30.nii.gz'
dti30_b0_brain = 'bet.b0.dti30.nii.gz'
dti30_b0_brain_mask = 'bet.b0.dti30_mask.nii.gz'

#
# DTI Correction
#

dti = fsl.DTIFit()
#dti.inputs.dwi = os.path.abspath(os.path.join(input_path, eddy_prefix + '.nii.gz'))
dti.inputs.dwi = '/gandg/infinite/imaging_data/individuals/inf0117/1/dti/input/eddy.nii.gz'
#dti.inputs.bvecs = os.path.join(input_path, eddy_prefix + 'rotated.bvecs')
dti.inputs.bvecs = '/gandg/infinite/imaging_data/individuals/inf0117/1/dti/input/eddy.eddy_rotated_bvecs'
dti.inputs.bvals = os.path.abspath(os.path.join(infinite_path, 'dti30.bval')) 
dti.inputs.base_name = output_path
dti.inputs.mask = dti30_b0_brain_mask
dti.cmdline
