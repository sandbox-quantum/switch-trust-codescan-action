# Switch Trust Inventory Scan

A [GitHub Action](https://github.com/features/actions) for using [Switch Trust](https://www.switchagents.ai/). This action performs static analysis on your code to detect AI assets (such as models, agents, and MCP servers), creating an inventory. This inventory is then sent to your Switch Trust instance, where it's enriched with additional information and analyzed for issues. You can view the results in the Switch Trust web interface. Refer to the [Switch Trust user guide](https://docs.switchagents.ai/) for details.

Scans appear in Switch Trust under the **GitHub** data source. If you also scan GitLab projects with the [Switch Trust Inventory Scan CI/CD component](https://gitlab.com/sandboxaq/switch-trust-codescan-workflow), those are reported separately, so the two inventories stay distinguishable.



## Configuration
You can configure the Action as shown in the following example::


```yaml
name: Example workflow for Python using Switch Trust Inventory Scan
on: push
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v5
      - name: Run Switch Trust Inventory detection
        uses: sandbox-quantum/switch-trust-codescan-action@v6
        with:
          switch_trust_instance: https://app.flintai.dev
          switch_trust_token: ${{ secrets.SWITCH_TRUST_TOKEN }}
          llm_model: anthropic:claude-opus-4-8
          llm_api_key: ${{ secrets.LLM_API_KEY }}
```

## Properties
Properties are passed to GitHub Action via explicit `with` input variables. For security, we strongly recommend using [GitHub secrets](https://docs.github.com/en/enterprise-cloud@latest/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets) for the sensitive input variables `switch_trust_token` and `llm_api_key`.


| Property            | Required | Description                                                                                                                              |
| ------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `switch_trust_instance`  | yes      | URL of your Switch Trust instance, e.g. `https://app.flintai.dev`      |
| `switch_trust_token`     | yes      | API key for your Switch Trust instance                                                                                                       |
| `llm_model`         | yes      | LLM to use, in the form `<provider>:<model>`. Supported providers: `anthropic`, `openai`, `gemini`/`google` (e.g. `anthropic:claude-opus-4-8`). |
| `llm_api_key`       | yes      | API key for the provider selected in `llm_model`. The action forwards it to the scanner under the provider-native name (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, or `GOOGLE_API_KEY`). |

## About
Scans the repository contents for AI usage and reports findings back to Switch Trust.

[Learn more](https://www.switchagents.ai/).
