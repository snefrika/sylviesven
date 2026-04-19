#overview
*Create documentation of bidding system to be used by Sylvie and Sven
*Start from the 2020.pdf

#Steps
*1NT opening
*2C and 2D opening
*do not copy 2H opening
*do not copy 1D opening
*intervention after 1m

#result should be a google docs file

#Generating the HTML file
*Edit oursystem.md, then run the following Python command to produce oursystem.html:
python3 - <<'EOF'
import markdown
import re

with open('/home/snef/Documents/bridge/sylviesven/oursystem.md', 'r') as f:
    content = f.read()

# Remove [WIP] sections and lines
filtered = []
skip_level = 0
for line in content.splitlines():
    m = re.match(r'^(#+)', line)
    if m:
        level = len(m.group(1))
        if '[WIP]' in line:
            skip_level = level
            continue
        elif skip_level and level <= skip_level:
            skip_level = 0
    if skip_level or '[WIP]' in line:
        continue
    filtered.append(line)
content = '\n'.join(filtered)

html_body = markdown.markdown(content, extensions=['tables'])
html_body = html_body.replace('♥', '<span style="color:red">♥</span>')
html_body = html_body.replace('♦', '<span style="color:red">♦</span>')

full_html = f"""<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<style>
  body {{ font-family: Arial, sans-serif; font-size: 11pt; max-width: 900px; margin: 40px auto; }}
  h1 {{ font-size: 18pt; }}
  h2 {{ font-size: 15pt; border-bottom: 1px solid #ccc; padding-bottom: 4px; }}
  h3 {{ font-size: 13pt; }}
  h4 {{ font-size: 11pt; }}
  table {{ border-collapse: collapse; margin: 8px 0; }}
  td, th {{ border: 1px solid #999; padding: 4px 10px; }}
  th {{ background: #f0f0f0; }}
  hr {{ border: none; border-top: 1px solid #ccc; }}
</style>
</head>
<body>
{html_body}
</body>
</html>"""

with open('/home/snef/Documents/bridge/sylviesven/oursystem.html', 'w') as f:
    f.write(full_html)
EOF
*Upload oursystem.html to Google Drive and open with Google Docs to preserve red ♥ and ♦ coloring