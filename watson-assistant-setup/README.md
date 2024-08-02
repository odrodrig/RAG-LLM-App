# Steps to Connect this Application to watsonx Assistant

You connect your assistant by using the api specification to add a custom extension.

### Download the api specification

Download the [rag-app-openapi.json](./rag-app-openapi.json) specification file. 

Use this specification file to create and add the extension to your assistant.

### Build and add extension

1.  In your assistant, on the **Integrations** page, click **Build custom extension** and use the `rag-app-openapi.json` specification file to build a custom extension named `rag-app`. For general instructions on building any custom extension, see [Building the custom extension](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-build-custom-extension#building-the-custom-extension).

1.  After you build the `rag-app` extension, and it appears on your **Integrations** page, click **Add** to add it to your assistant. For general instructions on adding any custom extension, see [Adding an extension to your assistant](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-add-custom-extension).

1.  In **Authentication**, choose **OAuth 2.0**. Select **Custom apikey** as the grant type in the next dropdown, and then copy and paste your IBM Cloud API key.

1.  In **Servers**, choose the region from which you would like to access the watsonx.ai service. `us-south` is selected as the default region. 

If you add apis and capabilities to this application, feel free to add them to the openapi specification. The application is intended to be an example of how to get started.

## Upload sample action

Use **Actions Global Settings** to upload the [`rag-app-actions.json`](./watsonx-actions.json) file in this directory to your assistant. For more information, see [Uploading](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-admin-backup-restore#backup-restore-import). You may also need to refresh the action **Preview** chat, after uploading, to get all the session variables initialized before these actions will work correctly.

1. Under **Variables/Created** by you within the Actions page, set the `watsonx_project_id` session variable using [a project ID value from watsonx](https://dataplatform.cloud.ibm.com/docs/content/wsj/manage-data/manage-projects.html?context=wx&audience=wdp).

**NOTE**: If you import the actions _before_ configuring the extensions, you will see errors on the actions because it could not find the extensions. Configure the extensions (as described [above](#prerequisites)), and re-import the action JSON file.

This utility includes [a JSON file with sample actions](./rag-app-actions.json) that are configured to use the `rag-app` extension.

| Action                        | Description                                                                                                                                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Invoke watsonx Generation API | Connects to watson.ai with the selected model and the model input |
| Invoke watsonx Generation Stream API | Connects to watsonx.ai with the selected model and model input and streams the response back to the user. |
| Test model                    | Simple test action that asks what model, length, temperature, prompt you want, asks if you want to stream the response and then calls "Invoke watsonx Generation API" or "Invoke watsonx Generation Stream API" based on if streaming was required so the model can generate a response to the specified prompt. |
| No Action Matches | This is created by watsonx Assistant, but for this starter kit it is configured to trigger the "Invoke watsonx Generation Stream API" as a sub-action using the defaults and the user input. |

Note that the "Test model" action includes a step that invokes an extension and includes a parameter named `model_id`. You can set the `model_id` session variable to control which model is used by `Test model`. You can see which models work with the Generate API by viewing the supported foundation models in [the watsonx Prompt Lab](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/fm-prompt-lab.html?context=wx).


### Session variables

These are the session variables used in this example.

- `model_id`: The ID of the watsonx model that you select for this action. Defaults to `ibm/granite-13b-chat-v2`.
- `model_input`: The input to the model.
- `model_parameters_max_new_tokens` : The maximum number of new tokens to be generated. Defaults to 256.
- `model_parameters_min_new_tokens`: The minimum number of the new tokens to be generated. Defaults to 1.
- `model_parameters_temperature` : The value used to control the next token probabilities. The range is from 0.05 to 1.00; 0 makes it _mostly_ deterministic.
- `model_parameters_repetition_penalty`: Represents the penalty for penalizing tokens that have already been generated or belong to the context. The range is 1.0 to 2.0 and defaults to 1.1.
- `model_parameters_stop_sequences`: Stop sequences are one or more strings which will cause the text generation to stop if/when they are produced as part of the output. Stop sequences encountered prior to the minimum number of tokens being generated will be ignored. The list may contain up to 6 strings. Defaults to ["\n\n"]
- `model_parameters_decoding_method`: The strategy used for picking the tokens during generation of the output text.
- `model_parameters_include_stop_sequence`: The value to control presence of matched stop sequences from the end of the output text.
- `model_parameters_random_seed`: A random number generator seed to use in sampling mode for experimental repeatability.
- `model_parameters_time_limit`: The amount of time in milliseconds to wait before stopping generation.
- `model_parameters_top_k`: The number of highest probability vocabulary tokens to keep for top-k-filtering.
- `model_parameters_top_p`: Similar to top_k except the candidates to generate the next token are the most likely tokens with probabilities that add up to at least top_p.
- `model_response`: The text generated by the model in response to the user input.
- `watsonx_api_version` - watsonx api date version. It currently defaults to `2023-05-29`.
- `watsonx_project_id`: You **MUST** set this value to be [a project ID value from watsonx](https://dataplatform.cloud.ibm.com/docs/content/wsj/manage-data/manage-projects.html). By default, this is a [sandbox project id](https://dataplatform.cloud.ibm.com/docs/content/wsj/manage-data/sandbox.html) that is automatically created for you when you sign up for watsonx.ai.

