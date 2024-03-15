# Gazeformer
Official codebase for CVPR 2023 publication "Gazeformer: Scalable, Effective and Fast Prediction of Goal-Directed Human Attention" by Sounak Mondal, Zhibo Yang, Seoyoung Ahn, Dimitris Samaras, Greg Zelinsky, Minh Hoai.

Contact **Sounak Mondal** at ```somondal@cs.stonybrook.edu``` for any queries.

[[arXiv](https://arxiv.org/abs/2303.15274)] [[CVPR 2023 Open Access](https://openaccess.thecvf.com/content/CVPR2023/html/Mondal_Gazeformer_Scalable_Effective_and_Fast_Prediction_of_Goal-Directed_Human_Attention_CVPR_2023_paper.html)] 

Predicting human gaze is important in Human-Computer Interaction (HCI). However, to practically serve HCI applications, gaze prediction models must be scalable, fast, and accurate in their spatial and temporal gaze predictions. Recent scanpath prediction models focus on goal-directed attention (search). Such models are limited in their application due to a common approach relying on trained target detectors for all possible objects, and the availability of human gaze data for their training (both not scalable). In response, we pose a new task called ZeroGaze, a new variant of zero-shot learning where gaze is predicted for never-before-searched objects, and we develop a novel model, Gazeformer, to solve the ZeroGaze problem. In contrast to existing methods using object detector modules, Gazeformer encodes the target using a natural language model, thus leveraging semantic similarities in scanpath prediction. We use a transformer-based encoder-decoder architecture because transformers are particularly useful for generating contextual representations. Gazeformer surpasses other models by a large margin on the ZeroGaze setting. It also outperforms existing target-detection models on standard gaze prediction for both target-present and target-absent search tasks. In addition to its improved performance, Gazeformer is more than five times faster than the state-of-the-art target-present visual search model.

# Installation

```bash
conda create --name gazeformer python=3.8.5
conda activate gazeformer
bash install.sh
```
# Files to download

Download the contents of this [link](https://drive.google.com/drive/folders/1uA6M-wtDrh_fqUgFLDzc1VzsGC-1I19U?usp=sharing). The contents are as follows

```
-- ./dataset
    -- resnet-features.tar.gz                     # pre-computed image features
    -- embeddings.npy                             # pre-computed task language features
    -- coco_search18_fixations_TP_train.json      # COCO-Search18 training fixations
    -- coco_search18_fixations_TP_validation.json # COCO-Search18 validation fixations
    -- coco_search18_fixations_TP_test.json       # COCO-Search18 test fixations
-- ./checkpoints                                  
    gazeformer_cocosearch_TP.pkg                  # checkpoint for inference on target-present test data
-- ./SemSS
    test_TP_Sem.pkl
    test_TA_Sem.pkl
    stuffthing_maps.zip
```

Extract ```resnet-features.tar.gz``` to obtain the pre-computed image features.

# Scripts

To train the model, run the following

``` python3 train.py --dataset_dir=<dataset-directory> --img_ftrs_dir=<precomputed-image-features-directory> --cuda=<device-id> ```

For evaluation, run the following

``` python3 test.py --trained_model=<model-checkpoint> --dataset_dir=<dataset-directory> --img_ftrs_dir=<precomputed-image-features-directory> --cuda=<device-id> ```

# Paper Updates

We have updated the implementation of SemSS and SemFED metrics. Please find the details and the updated SemSS and SemFED scores in the appendix (Section 6) at the end of the [arXiv](https://arxiv.org/abs/2303.15274) version of our paper. Gazeformer achieves state-of-the-art performance on the updated SemSS and SemFED metrics.

For Semantic Sequence Score, ```./SemSS/stuffthing_maps.zip``` file in the Google Drive link must be extracted to obtain the "segmentation_map_dir" directory.

To get semantic sequence score, use the following code in ```test.py```.

```
    if condition == 'present':
        with open('test_TP_Sem.pkl', "rb") as r:
            fixations_dict = pickle.load(r)
            r.close()
    elif condition == 'absent':
        with open('test_TA_Sem.pkl', "rb") as r:
            fixations_dict = pickle.load(r)
            r.close()
    sem_seq_score = get_semantic_seq_score(predictions, fixations_dict, max_len, segmentation_map_dir)
```

# Citation 
If you use this work, please cite as follows :
```
  @InProceedings{Mondal_2023_CVPR,
    author    = {Mondal, Sounak and Yang, Zhibo and Ahn, Seoyoung and Samaras, Dimitris and Zelinsky, Gregory and Hoai, Minh},
    title     = {Gazeformer: Scalable, Effective and Fast Prediction of Goal-Directed Human Attention},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2023},
    pages     = {1441-1450}
}
```


