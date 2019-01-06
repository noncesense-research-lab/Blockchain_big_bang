# r-blockchain-sim
Attempt to simulate blockchain dynamics  
2017 Gingeropolous  

## Features
- Overly commented code that makes fiddling quite easy, hopefully.  
- Variable transaction sizes.  
- Simulates backlogs of transactions in the txpool.  
- Uses oscillator function to mimic bursts and lulls in network usage. 

## How to use
First, install r. For ubuntu desktop, I recommend R studio. If you prefer to do everything all command line, follow these directions.  
```sudo add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu xenial/'```  
```sudo apt-get update```  
```sudo apt-get install r-base```  

## Running the script
If your in R studio, just hit ctrl-alt-R to run the code. If your in the command line, just use  
```Rscript dynamic_blocksize.R```  

The default run time is 1000 blocks. All of the parameters that should be modified are indicated in the code. The default is 1000 blocks. The script will create 4 files:  
1. blockchain.txt - the blockchain output, just with block "header" info
2. live_blockchain.txt - a live feed of the blockchain, with the block templates for each block 
3. final_txpool.txt - which transactions didn't get in
4. Rplots.pdf - plots of the histograms. This isn't actually useful, just cool I guess. 
