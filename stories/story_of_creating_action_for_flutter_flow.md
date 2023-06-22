# Story
For the purpose of working on an application created using the FlutterFlow platform, we needed to create a custom action using the Dart language and the Flutter framework. Since we haven't dealt with both the framework and the Dart language before, we decided to use ChatGPT to generate such an action, which will be able, using the input data we provide to it, to execute a request to the API based on the documentation in the OpenAPI format, and then based on the result of the request it will execute the appropriate action - in the case of a correct answer, it will be thrown into the application state, and otherwise the previously prepared value will be thrown into the application state.


## Prompt
```
Using this OpenAPI documentation https://raw.githubusercontent.com/openai/openai-openapi/master/openapi.yaml create code in flutter for action that will call "/chat/completions" endpoint. 
Use “gpt-3.5-turbo-16k” model. 
“Message” parameter value should be retrieved from AppState. 
To get AppState use “FFAppState” global method. 
List of messages is stored under “msgs” key in AppState and each element of that list has structure like this:

{
    role: String,
    content: String
}

Response should be added back to AppState to list of messages under `msgs` key. 
If any issue will occur, add “Coś poszło nie tak... Spróbuj ponownie lub odśwież stronę.” message to this list with “system” role. 
Action should return true if everything works as expected, and false if not.
```

## Response
```dart
import '/backend/backend.dart';
import '/backend/schema/structs/index.dart';
import '/flutter_flow/flutter_flow_theme.dart';
import '/flutter_flow/flutter_flow_util.dart';
import '/custom_code/actions/index.dart';
import '/flutter_flow/custom_functions.dart';
import 'package:flutter/material.dart';

import 'dart:convert';
import 'package:http/http.dart' as http;

Future<bool> callChatCompletionsAPI() async {
  final appState = FFAppState();
  final apiUrl = 'https://api.openai.com/v1/chat/completions';
  final model = 'gpt-3.5-turbo-16k';

  final message = appState.msgs
      .map((msg) => {
            'role': msg['role'],
            'content': msg['content'],
          })
      .toList();

  final requestBody = {
    'model': model,
    'messages': message,
  };

  final headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_KEY', // Replace with your OpenAI API key
  };

  try {
    final response = await http.post(
      Uri.parse(apiUrl),
      headers: headers,
      body: json.encode(requestBody),
    );

    if (response.statusCode == 200) {
      final jsonResponse = json.decode(response.body);
      final completions = jsonResponse['choices'][0]['message']['content'];

      // Update AppState with the response
      appState.msgs.add({
        'role': 'system',
        'content': completions,
      });

      return true;
    } else {
      print('API request failed with status code: ${response.statusCode}');
    }
  } catch (e) {
    print('An error occurred: $e');
  }

  // Handle error by adding a system message
  appState.msgs.add({
    'role': 'system',
    'content': 'Coś poszło nie tak... Spróbuj ponownie lub odśwież stronę.',
  });

  return false;
}
```
