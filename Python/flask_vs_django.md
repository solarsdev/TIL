# Flask vs Django

## Python 프레임워크

- 웹사이트를 만들때 일정 부분 서포트 (미리 코드가 작성되어 있음)를 담당

### Flask, Pyramid

- Flask와 Pyramid의 경우 미니멀함을 추구하고 있음
- 장점으로 빠른 시간에 무엇인가를 만들어낼 수 있지만, 단점으로는 많은 기능을 제공하지는 않음
- 일례로 피라미드나 플라스크에서 웹서버를 기동하기 위해서는 다음과 같은 단 몇줄만으로 서버의 기동이 가능

```python
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.response import Response

def hello_world(request):
    return Response('Hello World!')

if __name__ == '__main__':
    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        app = config.make_wsgi_app()
    server = make_server('0.0.0.0', 6543, app)
    server.serve_forever()
```

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

### Django

- 장고의 경우에는 거대한 프레임워크에 다양한 요소들이 미리 구성되어 있음
- 미니멀한 프레임워크를 쓰게 되면 결국 필요한 요소들을 어디선가 구해오거나 직접 만들어야 함
  - _you can focus on writing your app without needing to reinvent the wheel._
- 장고의 철학은 웹사이트를 만들때 필요한 요소들을 전부 장고 프레임워크에서 담당하고, 실제로 아이디어를 구현할 앱에 관련된 코드만을 개발자가 구현하도록 유도하는 것
- 유저 관리, 컨텐츠 관리등이 미리 포함되어 있기 때문에 웹사이트를 만들기 위해 최적화되어 있다고 생각됨

## Conclusion

- 웹사이트가 유저관리, 컨텐츠 관리를 필요로 한다면 Django 프레임워크를 사용할것을 고려해보자
- Django의 기능을 사용하던 중 점점 컨텐츠 관리 기능만으로는 부족해서 Django의 기능 자체를 수정하고 있다면 이미 범위를 벗어난 것일 수도 있음
- 이런 경우에는 미니멀한 프레임워크를 선정하여 원하는 기능을 입맛대로 개발해나가면서 사용하는것이 편리할 수도 있음
