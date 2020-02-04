# Flow

<div class="mermaid">
graph TD;
  subgraph Lambda
    lambda_request(("Request"))
    lambda_handler("app.lambda_handler()")
    lambda_request-->lambda_handler;

    lambda_validate_template("app.validate_template()")
    lambda_handler-->lambda_validate_template

    is_template_valid{"Is template valid?"}
    lambda_validate_template-->is_template_valid

    lambda_response(("Response"))
    is_template_valid -- no --> lambda_response

    upload_to_s3("upload_to_s3()")
    is_template_valid -- yes --> upload_to_s3

    get_session_credentials("app.get_session_credentials()")
    upload_to_s3-->get_session_credentials

    get_session_credentials-->lambda_response
  end

  subgraph validate_template
    validate_template_validate_template_format("validate_template_format()")
    lambda_validate_template-.->validate_template_validate_template_format
  end

</div>
