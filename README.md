# release-slack-action


Reusable workflow used for sending slack messages regarding the satus of a release.
For more information, see the official [docs](https://docs.github.com/en/actions/using-workflows/reusing-workflows)

# Install
We use two actions, to send the start and finish of the release.
The action needs to be installed the follow way.

>`SLACK_BOT_RELEASE_TOKEN` your slack app bot token.

>`YOUR_CHANNEL_TO_POST_MESSAGES` the channel to which you want the action to post the message to

```yaml
jobs:
  notify-start:
    name: Notify release start
    uses: ho-nl/release-slack-action/.github/workflows/notify-slack-start.yml@main
    secrets:
      slackToken: ${{ secrets.SLACK_BOT_RELEASE_TOKEN}}
    with:
      message: CUSTOM_MESSAGE (optional if no message is provided, the repository name will be used)
      channel: YOUR_CHANNEL_TO_POST_MESSAGES
      notifyOnlyOnFailure: boolean

# This would be your actual deployment job
  release:
    name: Release
    runs-on: ubuntu-latest


  notify-end:
    if: always()
    name: Notify release end
    needs: [notify-start, release]
    uses: ho-nl/release-slack-action/.github/workflows/notify-slack-end.yml@main
    secrets:
      slackToken: ${{ secrets.SLACK_BOT_RELEASE_TOKEN}}
    with:
      time: ${{ needs.notify-start.outputs.time }}
      result: ${{ needs.release.result }}
      message: CUSTOM_MESSAGE (optional if no message is provided, the repository name will be used)
      channel: YOUR_CHANNEL_TO_POST_MESSAGES
      notifyOnlyOnFailure: boolean
```

![image](https://github.com/ho-nl/release-slack-action/assets/5382391/fcc383c9-533f-4a17-8be5-8a5202cedffd)
![image](https://github.com/ho-nl/release-slack-action/assets/5382391/4d4ecc08-e9ff-4783-9bd3-4c61c81dabee)

