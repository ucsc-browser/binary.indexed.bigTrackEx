Here is some example [wig data](http://genome.ucsc.edu/FAQ/FAQformat.html#format6) that is variable step:

<pre>
variableStep chrom=chr1 span=1
368244       2
variableStep chrom=chr2 span=1
368250       2
</pre>

This can be loaded as a custom track when proceeded by a [custom track line](http://genome.ucsc.edu/goldenPath/help/customTrack.html#TRACK) and pasted in the Browser Custom Tracks page.

<pre>
track name=Dens type=bigWig description="Density of ZFBS_morph overlaps" visibility=full db=hg19 autoScale=off viewLimits=0.0:20 color=165,42,42 yLineMark=1 yLineonoff=on priority=100 
variableStep chrom=chr1 span=1
368244       2
variableStep chrom=chr2 span=1
368250       2
</pre>

Or a URL to a page that displays this text with the custom track line can be pasted in the browser as well, such as [this link](https://raw.githubusercontent.com/ucsc-browser/binary.indexed.bigTrackEx/master/simpleBigWig/customTrack.wigInput).

While this works for small files, if your file is very large, it may fail to upload to the Browser, and a better approach exists provided you can host your data on the internet. The alternative solution is to index the data and give the Browser a URL to the location of your indexed file. Then, as you browse your data, only the sections you are viewing will be requested from your indexed file and sent over the internet.  This will increase speed and prevent from having to upload your entire file.

To index this file you used the utility wigToBigWig.  It is available here:  http://hgdownload.soe.ucsc.edu/admin/exe/ Two copies are included in this directory too.

The input file should not include the trackline, see [wigInput](https://github.com/ucsc-browser/binary.indexed.bigTrackEx/blob/master/simpleBigWig/wigInput) in this directory.  The command requires knowing the size of the chromosomes you are going to load the data into, which can be pulled from the UCSC website at the hgdownload location. In the below example this is for hg19. 

<pre> 
./wigToBigWig_linux.x86_64 wigInput http://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/hg19.chrom.sizes out.bw
</pre>

The resulting out.bw is a binary indexed version of the text-based wigInput allowing the browser to access this data easily. Now the original custom track input can be shortened to a having a bigDataUrl parameter that points to where this data locates:

<pre>
track name=Dens_bw type=bigWig description="Density of ZFBS_morph overlaps" visibility=full db=hg19 autoScale=off viewLimits=0.0:20 color=165,42,42 yLineMark=1 yLineonoff=on priority=100 bigDataUrl=http://location/of/file/out.bw
</pre>

A URL to this text file can even be sent to the Browser shortening the step further. Such as pasting in this link: https://raw.githubusercontent.com/ucsc-browser/binary.indexed.bigTrackEx/master/simpleBigWig/customTrack.bigWig

Here is even a quick version of a URL pointing to the Browser that also loads this custom track:



http://genome.ucsc.edu/cgi-bin/hgTracks?db=hg19&position=chr9%3A368243-368248&hgct_customText=https://raw.githubusercontent.com/ucsc-browser/binary.indexed.bigTrackEx/master/simpleBigWig/customTrack.bigWig





