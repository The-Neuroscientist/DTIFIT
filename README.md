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
import shutil

#
# Create Environtment
#

# Sets working directory and calls the dti and input folders
# What is the first line changing my directory to?
subject_dir = os.getcwd()
dti_dir = os.path.abspath( os.path.join(subject_dir, 'dti'))
dti_input_dir = os.path.abspath( os.path.join(dti_dir, 'input'))

# Shortcuts to save time later
eddy_prefix    = 'eddy'
input_path      = '../dti/input'
# do I need this?
output_prefix   = ?
# Can I have my code create this directory or should I just make this directory in every patient?
output_path     = '../dti/dti/' 

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
dti.inputs.dwi = os.path.join(input_path, eddy_prefix + '.nii.gz')
dti.inputs.bvecs = os.path.join(input_path, eddy_prefix + 'rotated.bvecs')
dti.inputs.bvals = os.path.abspath(os.path.join(infinite_path, 'dti30.bval'))
# Is this the output? or should that be specified elsewhere?
dti.inputs.base_name = 'dtifit_'
# Shouldn't this be dti30_b0_brain_mask?
dti.inputs.mask = dti30_mask
dti.cmdline
