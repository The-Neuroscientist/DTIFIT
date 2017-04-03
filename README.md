# DTIFIT
# Creating a script to run dtifit
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

output_prefix    = 'eddy'
output_path      = '../methods/eddy'

dti30 = 'dti30.nii.gz'
dti30_b0_brain = 'bet.b0.dti30.nii.gz'
dti30_b0_brain_mask = 'bet.b0.dti30_mask.nii.gz'

#
# Specify Inputs
#

# 
# Specify Output
#

#
# DTI Correction
#
dti = fsl.DTIFit()
dti.inputs.dwi = os.path.join(output_path, eddy_prefix + '.nii.gz')
dti.inputs.bvecs = os.path.join(output_path, eddy_prefix + 'rotated.bvecs')
dti.inputs.bvals = os.path.abspath(os.path.join(infinite_path, 'dti30.bval'))
dti.inputs.base_name = 'dtifit_'
dti.inputs.mask = dti30_mask
dti.cmdline
