**For the inital setup of UMI environment, follow the UMI Gripper github page steps found here: https://github.com/real-stanford/universal_manipulation_interface**

# Training limited_data_set
**Step 1:** Run the followiung commands in a new terminal:   
```conda activate umi```   
```cd `~/universal_manipulation_interface-main```   

**Step 2:** Whilst inside the ```universal_manipulation_interface-main directory```, create a new folder called "limited_data_set".   
Record your limited dataset and extract the files into this folder.   
***NOTE***: Ensure you have required recordings. Steps to this can be found through the origninal UMI Gripper github here: https://swanky-sphere-ad1.notion.site/UMI-Data-Collection-Instruction-4db1a1f0f2aa4a2e84d9742720428b4c   

**Step 3:** Remain in the ```universal_manipulation_interface-main directory``` directory.   
Run the pipeline with the new dataset using the following command:   
```python run_slam_pipeline.py limited_data_set```   

**Step 4:** Generate the dataset for training:   
```python scripts_slam_pipeline/07_generate_replay_buffer.py -o limited_data_set/dataset.zarr.zip limited_data_set``` 

**IMPORTANT**: The following step was completed on a RTX 4070TI 12GB which is different to the original UMI Gripper page.   
If you are using the desired RTX 3090 24GB please follow the step for training data here: https://github.com/real-stanford/universal_manipulation_interface   
If you are using the RTX 4070TI 12GB, you may proceed with the following step:

**Step 5:** Train the data. 
```python train.py --config-name=train_diffusion_unet_timm_umi_workspace task.dataset_path=example_demo_session/dataset.zarr.zip policy.obs_encoder._target_=diffusion_policy.model.vision.timm_obs_encoder.TimmObsEncoder policy.obs_encoder.model_name=resnet18 policy.obs_encoder.pretrained=False```   

**Now close the terminal**   

**Open a new terminal (temrinal 1 and complete the following)**:   
***DO NOT activate UMI environment***   
**Step 6:** Edit umi_replay_node.py by running this command.   
```nano ~/universal_manipulation_interface-main/ros2_ws/src/umi_visualizer/umi_visualizer/umi_replay_node.py```   
Look for the line of code ***dataset_path = Path.home() / 'universal_manipulation_interface-main' / '....' / 'dataset.zarr.zip'***   
Change the '....' to the limited_data_set folder. You should have:   
***dataset_path = Path.home() / 'universal_manipulation_interface-main' / 'limited_data_set' / 'dataset.zarr.zip'***
