Install labctl CLI:
curl -sf https://labs.iximiuz.com/cli/install.sh | sh

Authorize labctl:
labctl auth login

Pull course content:
labctl content pull course ansi-ada-snake-34d534e7

Stream your changes:
labctl content push -fw course ansi-ada-snake-34d534e7
