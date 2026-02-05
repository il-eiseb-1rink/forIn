# cctv-anomaly

## Flagging odd behaviour in surveillance feeds
### Why it is worth building
- A service where a model watches camera footage and calls out abnormal events
- Smart cameras let a small crew cover more ground, and turn incident analysis into something automated
- - -
### Material used
- Abnormal-behaviour clips released by a city government through the public AI dataset portal
- A well-known anomaly-detection clip collection with both normal and abnormal sequences
- - -
### Preparing the data
![image](https://user-images.githubusercontent.com/66463059/102057232-f84bc680-3e30-11eb-847b-7d19dbc4122d.png)
- Every clip (.mp4) is chopped into chunks of 16 frames and turned into fc6-1 vectors.
- Those vectors are averaged and normalised into a C3D feature table.
- - -
### Training
##### Following the architecture from a 2018 surveillance-video paper
![image](https://user-images.githubusercontent.com/66463059/102055494-f03ed100-3e2e-11eb-9679-1691b4f70c99.png)
- The network follows that paper's design.
- One clip becomes 32 segments of C3D features placed in a bag, and each segment is a bag instance.
- Each instance is reduced to shape (1,4096) by mean and normalisation, then scored per bag instance.
- Judgement comes from comparing the top score in a positive bag against the top score in a negative bag: a higher positive score means the call was right, a higher negative one means it was wrong.
- A loss function drives the loop, shrinking total loss and raising classification accuracy.
- - -
### Result
![image](https://user-images.githubusercontent.com/66463059/102055902-e23d0680-3e2e-11eb-8044-e2367c153197.png)
- A small web front end was wired up so a user can upload footage, see whether anything abnormal appears, and get the moment it happened highlighted on the timeline. It was never deployed publicly.
- - -
### Further reading
1. The original anomaly-detection CVPR 2018 implementation
2. A university human-behaviour dataset companion repo