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

subject_dir = os.getcwd()
dti_dir = os.path.abspath( os.path.join(subject_dir, 'dti'))
dti_input_dir = os.path.abspath( os.path.join(dti_dir, 'input'))

eddy_prefix    = 'eddy'
input_path      = '../dti/input'

output_basename = os.path.abspath(os.path.join(dti_dir, output_path, output_prefix))

infinite_path = os.path.join(os.getenv('INFINITE_PATH'), 'infinite')

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
dti.inputs.base_name = 'dtifit_'
dti.inputs.mask = dti30_mask
dti.cmdline
