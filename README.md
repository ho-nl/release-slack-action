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
      channel: YOUR_CHANNEL_TO_POST_MESSAGES

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
      channel: YOUR_CHANNEL_TO_POST_MESSAGES
```
