#!/usr/bin/python

# MIT License

# Copyright (c) 2018 Raphael Hussung

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE. 

import os
 
import re
import sys, getopt
 
def main(argv):
    rootdir=('.')
    searched_class = argv[0]  #r'project\.project'
    searched_method = argv[1] #'onchange_partner_id'
    reg = r".*(?:_inherit|_name)[\s]{0,}=[\s]{0,}[\"\']" + re.escape(searched_class) + r'[\"\'].*'
    class_reg = r".*class[\s]{1,}(.*)\([\s]{0,}.*[\s]{0,}"
    def_reg = r'.*def[\s]{0,}(.*)[\s]{0,}\(.*'
    
    for folder, dirs, files in os.walk(rootdir):
        for file in files:
            if file.endswith('.py'):
                fullpath = os.path.join(folder, file)
                #print file
             
                class_stack_top = None
                with open(fullpath, 'r') as f:
                    file_contains_odoo_object = False
                    # look for _inherit match of requested odoo-object
                    for line in f:
                        # match for python-class name and remember last seen
                        matchClass = re.match(class_reg, line)
                        # match for odoo-object
                        matchObj = re.match(reg, line)
                        if matchClass:
                            class_stack_top = matchClass.group(1)
                        if matchObj:
                            file_contains_odoo_object = True
                            break
                    f.seek(0)
                    # iterate again over file, if odoo object was found and search for method within python class
                    if file_contains_odoo_object:
                        within_class = False
                        for i, line in enumerate(f):
                            matchClass = re.match(class_reg , line)
                            if matchClass and matchClass.group(1) == class_stack_top:
                                # searched class reached
                                within_class = True
                            elif matchClass and not matchClass.group(1) == class_stack_top and within_class:
                                # searched class left
                                within_class = False
                            else:
                                if within_class:
                                    matchMethod = re.match(def_reg, line)
                                    if matchMethod and matchMethod.group(1) == searched_method:
                                        print fullpath
                                        print str(i) + line
 
if __name__ == "__main__":
    main(sys.argv[1:])
