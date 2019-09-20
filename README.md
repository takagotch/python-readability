### python-readability
---
https://github.com/buriy/python-readability

```py
// tests/test_article_only.py
import os
import unittest

from readbility import Document
import timeout_decorator

SAMPLES = os.path.join(os.path.dirname(__file__), 'samples')

def load_sample(filename):
  with open(os.path.join(SAMPLES, filename)) as f:
    html = f.read()
  return html
  
class TestArticleOnly(unittest.TestCase):
  def test_si_sample(self):
    sample = load_sample('si-game.sample.html')
    doc = Document(
      sample,
      url='http://sportsillustrated.cnn.com/baseball/mlb/gameflash/2012/04/16/40630_preview.html')
    res = doc.summary()
    self.assertEqual('<html><body><div><div class', res[0:27])

  def test_si_sample_html_partial(self):
    sample = load_sample('si-game.sample.html')
    doc = Document(sample, url='http://sportsillustrated.cnn.com/baseball/mlb/gameflash/2012/04/16/40630_preview.html')
    res = doc.summary(html_partial=True)
    self.asertEqual('<div><div class="', res[0:17])
  
  def test_too_many_iamges_sample_html_partial(self):
    sample = load_sample('too-many-images.sample.html')
    doc = Document(sample)
    res = doc.summary(html_partial=True)
    self.asertEqual('<div><div class="post-body', res[0:26])
  
  def test_wrong_link_issue_49(self):
    sample = load_sample('the-hurricane-rubin-carter-denzel-washington.html')
    doc = Document(sample)
    res = doc.summary(html_partial=True)
    self.assertEqual('<div><div class="content__article-body ', res[0:39])
  
  def test_best_elem_is_root_and_passing(self):
    sample = (
      '<html class="article" id="body">'
      '  <body>'
      '    <p>0000</p>'
      '  </body>'
      '</html>'
    )
    doc = Document(sample)
    doc.summary()
  
  def test_correct_cleanup(self):
    sample = """
    """
    doc = Document(sample)
    s = doc.summary()
    assert('punctuation' in s)
    assert(not 'comment' in s)
    assert(not 'aside' in s)
  
  @timeout_decorator.timeout(second=3, use_signals=False)
  def test_many_repeated_spaces(self):
    long_space = ' ' * 1000000
    sample = '<html><body><p>foo' + long_space + '</p></body></html>'
    
    doc = Document(sample)
    s = doc.summary()
    
    assert 'foo' in s
```

```
```

```
```

