##Requires Algorithmia.com account
##Algorithmia CLI: https://github.com/algorithmiaio/algorithmia-cli

##Note: Video processing is slow at the time

##Split video into frames that is read from Data API
algo run media/VideoAlgorithms -d '{"format":"SplitToFrames","data":{"input":{"inputVideoUrl":"data://.algo/youtube/DownloadYoutubeVid/temp/video.mp4"},"output":{"collection":"data://.my/filter","prefix":"algo_diego","extension":"jpg","zippedOutput":false},"fps":20}}'

##Apply deep filter to every frame in collection in Data API
for file in $(algo ls .my/filter); do
    algo run deeplearning/DeepFilter -d "{ \"images\": [\"data://.my/filter/$file\"], \"savePaths\" : [\"data://diego/filter_output/$file\"], \"filterName\": \"post_modern\"}"
done

##Combine frames to produce video from collection in Data API
algo run media/VideoAlgorithms -d '{"format":"CombineFromFrames", "data":{"input":{"uuids":["c8807fa2b7ed430fadb83afe595f00bf"], "inputCollections":["data://.my/filter_output_3"] }, "output":{"collection":"data://.my/videofilters", "prefix":"diego_style_3", "extension":"mkv", "zippedOutput":false }, "fps":20 } }'
