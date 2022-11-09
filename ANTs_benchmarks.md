
# MalfLabel

git clone https://github.com/ntustison/MalfLabelingExample
cd MalfLabelingExample

ARM:
time docker run -v $PWD:/data -w /data rkp1/ants-arm:latest /bin/bash malfCommand.sh | tee out

AMD:
time docker run -v $PWD:/data -w /data rkp1/ants-x86:2.3.4 /bin/bash malfCommand.sh | tee out

# antsBrainExtraction
git clone https://github.com/ntustison/antsBrainExtractionExample
cd antsBrainExtractionExample/

ARM:
time docker run -v $PWD:/data -w /data rkp1/ants-arm:latest /bin/bash antsBrainExtractionCommand.sh | tee out

AMD

time docker run -v $PWD:/data -w /data rkp1/ants-x86:2.3.4 /bin/bash antsBrainExtractionCommand.sh | tee out

# Template Building
git clone https://github.com/ntustison/TemplateBuildingExample
cd TemplateBuildingExample

ARM:
time docker run -v $PWD:/data -e ANTSPATH="/opt/ants-2.3.4" -w /data rkp1/ants-arm:latest /bin/sh templateCommandSyN.sh | tee out

AMD:
time docker run -v $PWD:/data -e ANTSPATH="/opt/ants-2.3.4" -w /data rkp1/ants-x86:2.3.4 /bin/sh templateCommandSyN.sh | tee out

