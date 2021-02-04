# Pulling

from requests_html import HTMLSession
import pandas as pd
import numpy as np
import re

session = HTMLSession()
r = session.get('https://www.clearancejobs.com/jobs?PAGE=2000')
r.html.render()


indx = [m.start() for m in re.finditer('\nR\nRecruiter\n', r.html.text)]

indx

indx_s = [None] * (len(indx) - 1)
numPosts = range(0, len(indx) - 1)
fields = [None] * (len(indx) - 1)


for i in numPosts:

  indx_s[i] = [m.start() for m in re.finditer('\n', r.html.text[indx[i]:indx[i+1]])]
  indx_s[i] = [x+indx[i] for x in indx_s[i]]
  fields[i] = len(indx_s[i]) - 1

indx_s[(len(indx) - 2)] = np.append(indx_s[(len(indx) - 2)],indx[len(indx)-1])
numFields = range(1,max(fields))

field_string = [['']*max(fields)]*(len(indx)-1)

for i in numPosts:

  for j in numFields:
    
    field_string[i][j] =  r.html.text[indx_s[i][j]:indx_s[i][j+1]]
    print(r.html.text[indx_s[i][j]:indx_s[i][j+1]])

