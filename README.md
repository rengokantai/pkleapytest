####pkleapytest
#####Chapter 2. Working with doctest
######The doctest language
```
python -m doctest text.txt
```
######Embeddingv(must py extension)
verbose output
```
python -m doctest  -v text.py
```
#####
#######
```
>>> from unittest.mock import Mock,call
>>> mock=Mock()
>>> mock.x
<Mock name='mock.x' id='36893488'>
>>> mock.x
<Mock name='mock.x' id='36893488'>
>>> mock.method_calls
[]
>>> mock.x("a",1,1)
<Mock name='mock.x()' id='36895632'>
>>> mock.method_calls
[call.x('a', 1, 1)]
>>> mock.assert_has_calls([call.x("a",1,1)])

```
other methods
```
>>> mock.mock_calls.index(call.x('a',1,1))
0
>>> mock.z.hello().start(1)
<Mock name='mock.z.hello().start()' id='37209584'>
>>> mock.mock_calls.index(call.z.hello())
1
>>> mock.mock_calls.index(call.z.hello().start(1))
2
>>> mock.mock_calls
[call.x('a', 1, 1), call.z.hello(), call.z.hello().start(1)]
```
return_value and side_effect
```
>>> mock.a.return_value='a'
>>> mock.a()
'a'
>>> mock.a('anystr')
'a'
>>> mock.b.side_effect=[1,2]
>>> mock.b()
1
>>> mock.b()
2
```

MagicMock
```
>>> from unittest.mock import create_autospec
>>> x = Exception('a','b')
>>> y = create_autospec(x)
>>> isinstance(y,Exception)
True
>>> from unittest.mock import MagicMock
>>> mock = MagicMock()
>>> 1 in mock
False
>>> mock.mock_calls
[call.__contains__(1)]
>>> mock.__contains__.return_value = True
>>> 2 in mock
True
>>> mock.mock_calls
[call.__contains__(1), call.__contains__(2)]
```

PropertyMock
```
>>> from unittest.mock import PropertyMock
>>> mock = Mock()
>>> prop = PropertyMock()
>>> type(mock).p=prop
>>> mock.p
<MagicMock name='mock()' id='37334064'>
>>> prop.mock_calls
[call()]
>>> mock.p=2
>>> prop.mock_calls
[call(), call(2)]
>>> type(Mock()) is type(Mock())
False
```

mock_open

```
>>> from unittest.mock import mock_open
>>> open = mock_open(read_data='a')
>>> with open('/fake/a.txt','r') as f:
...     print(f.read())
...
a
```

patch (better)
```
>>> from unittest.mock import patch, mock_open
>>> with patch('builtins.open', mock_open(read_data = 'moose')) as mock:
...     with open('/fake/file.txt', 'r') as f:
...             print(f.read())
...

>>> open
<MagicMock name='open' spec='builtin_function_or_method' id='37348816'>
```
io.BytesIO
```
>>> import io
>>> with patch('io.BytesIO'):
...     x = io.BytesIO(b'a')
...     io.BytesIO.mock_calls
...
[call(b'a')]
```