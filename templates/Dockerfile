#jinja2: trim_blocks: True, lstrip_blocks: True
# {{ ansible_managed }}
# {{ template_fullpath | regex_replace('^' ~ role_path, role_path | basename) }}

FROM certbot/certbot AS certbot

{% if 'dns-gandi' in certbot_authenticators %}
# dns-gandi dependencies
RUN pip install certbot-plugin-gandi
# https://github.com/obynio/certbot-plugin-gandi/issues/49
RUN pip install six
{% endif %}
