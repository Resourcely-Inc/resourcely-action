This is repo is to geernate the resourcely github action in the marketplace.

To use this github action, you can specify like the following example

```
      - uses: actions/resourcely-action@v1
        with:
          gh_access_token: ${{ secrets.GH_ACCESS_TOKEN }}
          tf_api_token: ${{ secrets.TF_API_TOKEN }}
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN_DEV }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          resourcely_api_host: "https://api.resourcely.io"
          
 ```

Note: `tf_api_token` is an optional parameter; if specified, it will go to terraform cloud to fetch terraform plan. If not , it will expect terraform plan already exist in the directory.