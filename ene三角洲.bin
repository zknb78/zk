#!/system/bin/sh

set -eu

script_dir=$(cd "$(dirname "$0")" && pwd -P)

card_key=""
if [ "${1:-}" = "-k" ] && [ -n "${2:-}" ]; then
  card_key=$2
  shift 2
fi

if [ -z "$card_key" ]; then
  printf '请输入卡密: '
  IFS= read -r card_key
fi

if [ -z "$card_key" ]; then
  echo "卡密不能为空" >&2
  exit 1
fi

set -- "$script_dir"/*.so
if [ "$1" = "$script_dir/*.so" ]; then
  echo "No .so file found beside this script." >&2
  exit 1
fi

if [ "$#" -ne 1 ]; then
  echo "More than one .so file found beside this script:" >&2
  for so_file in "$@"; do
    echo "  $so_file" >&2
  done
  exit 1
fi

so_file=$1
chmod 700 "$so_file" 2>/dev/null || true
echo "已启动"
"$so_file" -k "$card_key" --record-mirror
