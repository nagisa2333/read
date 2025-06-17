# 气源代码
气源相关模型以及推理代码已相对完善，本文档将详细介绍代码使用以及修改方法。

现在有两组代码都可以跑这个任务，
## 方法一：气源系统大模型
/nfsData/nfsShare/LPX/Aircraft-Large-Model 代码的使用方法如下：
### 训练流程：
1. 需要训练新模型时，把--is_training 设置为1，目前默认checkpoint为已训练好的919模型；
2. 需要更新数据集时把/nfsData/nfsShare/LPX/Aircraft-Large-Model/data_provider/data_config/train.yaml 中的data_path换成新的训练数据的文件夹，（注意文件夹内的文件需要归一化，归一化的代码在/nfsData/nfsShare/LPX/Aircraft-Large-Model/PHM_dataset/standardscaler.py 中，经过这个处理之后的文件夹可以直接输入模型）
3. 在run.py中可以调整超参也可以默认，--model_id 是模型的名字，后面eval要用，运行之后就会保存在checkpoints里。
4. 训练完成后，找到保存的checkpoint，把里面.pth文件的路径替换到run.py的超参--pretrained_weight 中，然后把--is_training设置为0，开启eval，其中你想要对哪些文件跑eval画图，在run.py的最后面
训练时如下：
<img width="615" alt="image" src="https://github.com/user-attachments/assets/7b7b4137-05bc-4427-a624-e0218171b16e" />

### 推理流程：
exp.fault_analysis(data_path='Aircraft-Large-Model/PHM_dataset/314test',setting=setting,folder_path='Aircraft-Large-Model/res') 这里改，data_path是你要eval的数据文件夹，folder_path是你要保存的图片的文件夹。
同样这里的数据也是要经过归一化处理，流程和训练数据一样

## 方法二：单飞机时序模型（目前919数据量够了，用这方法更快捷，方便）
/nfsData/nfsShare/LPX/sfy 代码的使用方法如下：
/nfsData/nfsShare/LPX/sfy/main_crossformer.py中，所有参数已经调整好，
直接运行就可以eval，只需要改eval函数中的data_path，数据同样经过前面的函数进行归一化。

如果需要重新训练，那么需要把

<img width="615" alt="image" src="https://github.com/user-attachments/assets/f913dd11-41e3-41a9-943c-4c274b41bb3d" />

注释的代码取消注释，/nfsData/nfsShare/LPX/sfy/data_config.py 在这里data更改训练数据位置。
<img width="615" alt="image" src="https://github.com/user-attachments/assets/b7c21e5e-1718-453d-9eb1-a650e965a252" />
