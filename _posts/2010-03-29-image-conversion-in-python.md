---
layout: post
title: Image Conversion in Python
date: '2010-03-29 16:29:00'
---

Yesterday I had to convert a lot of pictures to different formats. I quickly came up with a Python Script that searches a specified path recursively for images and converts them to one or more output format. It's nothing fancy, and it was late at night so don't blame me for bad code. I just thought I'd share it here because it might be useful for someone. In order to run this script you need to have [PIL](http://www.pythonware.com/products/pil/) installed.

```language-python
import os
import sys
import Image
class ImageConverter(object):  	
	def __init__(self, typesin, typesout):
		"""constructor"""
		self.typesin = typesin
		self.typesout = typesout
        
	def SearchFiles(self, path):
		if path[-1] != '/':
			path += '/'
		
		print "searching files in " + path
		for root, dirs, files in os.walk(path):
			for fname in files:
				fullpath = os.path.join(root, fname)
				if os.path.splitext(fname)[1] in self.typesin:
					self.ConvertImage(fullpath)
                    
	def ConvertImage(self, filepath):
		for t in self.typesout:
			outfile = os.path.splitext(filepath)[0] + t.lower()
			print "converting image %s to %s" % (filepath, outfile)	
			
			try:
				im = Image.open(filepath)
				im.save(outfile)
			except IOError as detail:
				print "failed to convert file %s" % (filepath)
			
			except:
				print "Unexpected error:", sys.exc_info()[0]
				raise
if __name__ == '__main__':
	if len(sys.argv) == 1 or len(sys.argv) > 2:
		print 'Usgae: python ' + str(sys.argv[0]) + ' /path/to/search/'
	else:
		converter = ImageConverter(['.gif',], ['.tiff',])
		converter.SearchFiles(sys.argv[1])
```
    
I think the script is pretty self-explanatory. Simply pass the path to search for images as a command line argument. When instantiating the class, you can pass the input and output formats as a list.

I think that's it for today.