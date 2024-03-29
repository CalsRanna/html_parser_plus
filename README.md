# Html Parser

This is a package use to parse html document into nodes and string.

## Install

```bash
flutter pub add html_parser_plus
```

## Getting started

```dart
import 'package:html_parser_plus/html_parser_plus.dart';

void main() {
  const String htmlString = '''
      <html lang="en">
      <body>
      <div><a href='https://github.com/simonkimi'>author</a></div>
      <div class="head">div head</div>
      <div class="container">
          <table>
              <tbody>
                <tr>
                    <td id="td1" class="first1">1</td>
                    <td id="td2" class="first1">2</td>
                    <td id="td3" class="first2">3</td>
                    <td id="td4" class="first2 form">4</td>

                    <td id="td5" class="second1">one</td>
                    <td id="td6" class="second1">two</td>
                    <td id="td7" class="second2">three</td>
                    <td id="td8" class="second2">four</td>
                </tr>
              </tbody>
          </table>
      </div>
      <div class="end">end</div>
      </body>
      </html>
      ''';
  final parser = HtmlParser();
  var node = parser.parse(htmlString);
  parser.query(node, '//div/a@text');
  parser.query(
    node,
    '//div/a/@href|dart.replace(https://,)|dart.substring(0,10)',
  );
  parser.queryNodes(node, '//tr/td|dart.sublist(0,2)');

  const String jsonString = '''
      {"author":"Cals Ranna","website":"https://github.com/CalsRanna","books":[{"name":"Hello"},{"name":"World"},{"name":"!"}]}
      ''';

  node = parser.parse(jsonString);
  parser.query(node, r'$.author');
  parser.query(
    node,
    r'$.website|dart.replace(https://,)|dart.substring(0,10)',
  );
  parser.queryNodes(node, r'$.books|dart.sublist(0,2)');
}


```

## Usage

So far, we have supported:

- some **xpath** syntax by **xpath_selector**
- almost all **jsonPath** syntax by **json_path**

And seven supported functions list below:

- **sublist** for `List<HtmlParserNode>`
- **replace** for `String`
- **replaceFirst** for `String`
- **replaceRegExp** for `String`
- **substring** for `String`
- **trim** for `String`
- **interpolate** for `String`, use `{{string}}` to indicate the piped value
- **match** for `String`

> You should know that in the function, the params **CAN NOT** be wrapped by **'** or **"**.

> And the rule is like **//div/a@text|dart.replace(Author,)|dart.replace( ,)|dart.interpolate(作者：{{string}})**.

> Use **|** to pipe all rules.

## Legacy

- `function:` will be replaced by `dart.` in the future.
