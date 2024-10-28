# Telegraf Output Plugin for Amazon Kinesis Data Firehose

[![](https://img.shields.io/github/license/muhlba91/telegraf-output-kinesis-data-firehose?style=for-the-badge)](LICENSE)
[![](https://img.shields.io/github/actions/workflow/status/muhlba91/telegraf-output-kinesis-data-firehose/verify.yml?style=for-the-badge)](https://github.com/muhlba91/telegraf-output-kinesis-data-firehose/actions/workflows/verify.yml)
[![](https://img.shields.io/coveralls/github/muhlba91/telegraf-output-kinesis-data-firehose?style=for-the-badge)](https://github.com/muhlba91/telegraf-output-kinesis-data-firehose/)
[![](https://api.scorecard.dev/projects/github.com/muhlba91/telegraf-output-kinesis-data-firehose/badge?style=for-the-badge)](https://scorecard.dev/viewer/?uri=github.com/muhlba91/telegraf-output-kinesis-data-firehose)
[![](https://img.shields.io/github/release-date/muhlba91/telegraf-output-kinesis-data-firehose?style=for-the-badge)](https://github.com/muhlba91/telegraf-output-kinesis-data-firehose/releases)
[![](https://img.shields.io/github/downloads/muhlba91/telegraf-output-kinesis-data-firehose/total?style=for-the-badge)](https://github.com/muhlba91/telegraf-output-kinesis-data-firehose/releases)
<a href="https://www.buymeacoffee.com/muhlba91" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="28" width="150"></a>

---

**:warning: Attention: first, and foremost, this plugin is developed for personal use. In fact, it is not - and will not be - fully production ready!**

---

This plugin makes use of the [Telegraf Output Execd plugin](https://github.com/influxdata/telegraf/tree/master/plugins/outputs/execd).
It will batch up Points in one Put request to Amazon Kinesis Data Firehose.

The plugin also provides optional common formatting options, like normalizing keys and flattening the output.
Such configuration can be used to provide data ingestion without the need of a data transformation function.

It expects that the configuration for the output ship data in line format.

---

## About Amazon Kinesis Data Firehose

It may be useful for users to review Amazons official documentation which is
available [here](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html).

## Usage

- Download the [latest release package](https://github.com/muhlba91/telegraf-output-kinesis-data-firehose/releases/latest) for your platform.

- Unpack the build to your system:

```bash
mkdir /var/lib/telegraf/firehose
chown telegraf:telegraf /var/lib/telegraf/firehose
tar xf telegraf-output-kinesis-data-firehose-<LATEST_VERSION>-<OS>-<ARCH>.tar.gz -C /var/lib/telegraf/firehose
# e.g. tar xf telegraf-output-kinesis-data-firehose-v1.0.0-linux-amd64.tar.gz -C /var/lib/telegraf/firehose
```

- Edit the plugin configuration as needed:

```bash
vi /var/lib/telegraf/firehose/plugin.conf
```

- Add the plugin to `/etc/telegraf/telegraf.conf` or into a new file in `/etc/telegraf/telegraf.d`:

```toml
[[outputs.execd]]
  command = [ "/var/lib/telegraf/firehose/telegraf-output-kinesis-data-firehose", "-config", "/var/lib/telegraf/firehose/plugin.conf" ]
  data_format = "influx"
```

- Restart or reload Telegraf.

## AWS Authentication

This plugin uses a credential chain for Authentication with the Amazon Kinesis Data Firehose API
endpoint. The plugin will attempt to authenticate in the following order:

1. web identity provider credentials via STS if `role_arn` and
   `web_identity_token_file` are specified,
2. assumed credentials via STS if the `role_arn` attribute is specified (source
   credentials are evaluated from subsequent rules),
3. explicit credentials from the `access_key`, and `secret_key` attributes,
4. shared profile from the `profile` attribute,
5. environment variables,
6. shared credentials, and/or
7. the EC2 instance profile.

If you are using credentials from a web identity provider, you can specify the
session name using `role_session_name`.
If left empty, the current timestamp will be used.

### AWS IAM Policy

The required AWS IAM Policy is:

```json
{
    "Statement": [
        {
            "Action": [
                "firehose:PutRecordBatch",
                "firehose:PutRecord"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:firehose:eu-west-1:126125163971:deliverystream/aws-firehose-homeassistant-stream-c49c07d",
            "Sid": ""
        },
        {
            "Action": "firehose:DescribeDeliveryStream",
            "Effect": "Allow",
            "Resource": "*",
            "Sid": ""
        }
    ],
    "Version": "2012-10-17"
}
```

## Configuration

See the file [`plugin.conf`](plugin.conf) as an example configuration file.

---

## Notes

The plugin was inspired by the [Amazon Kinesis Data Stream Output Plugin](https://github.com/morfien101/telegraf-output-kinesis).

## Supporting

If you enjoy the application and want to support my efforts, please feel free to buy me a coffe. :)

<a href="https://www.buymeacoffee.com/muhlba91" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="75" width="300"></a>
