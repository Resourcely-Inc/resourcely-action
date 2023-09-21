This is repo is to geernate the resourcely github action in the marketplace.

To use this github action, you can specify like the following example

```
      - uses: Resourcely-Inc/resourcely-action@test_action
        with:
          GH_ACCESS_TOKEN: ${{ secrets.GH_ACCESS_TOKEN }}
          TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}
          RESOURCELY_API_TOKEN: ${{ secrets.RESOURCELY_API_TOKEN_DEV }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          RESOURCELY_API_HOST: "https://api.dev.resourcely.io"
 ```