# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>

_os="$( \
  uname \
    -o)"
_dl_agent="true"
_retroarch="false"
_gearboy="true"
_platform="gameboy"
_emulators=()
if [[ "${_os}" == "Android" ]]; then
  _emulator="retroarch"
  _retroarch="true"
  _emulators+=(
    "${_emulator}"
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _emulator="gearboy"
  _emulators+=(
    "${_emulator}"
  )
fi
_archive="false"
_evmfs="true"
_app_id="com.nintendo.PokemonRed"
_uuid="DMG-APAU-0"
_title="Pokémon Red Version"
_rom_filename="${_uuid}"
_pkg=pokemon-red
pkgname="${_pkg}"
pkgver=1.0
pkgrel=1
_pkgdesc=(
  "1996 role-playing video games (RPGs)"
  "developed by Game Freak and published"
  "by Nintendo for the Game Boy."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
url="https://en.wikipedia.org/wiki/${_title}"
depends=(
  "${_emulators[@]}"
  "videogame-launcher"
)
if [[ "${_retroarch}" == "true" ]]; then
  depends+=(
    "libretro-gearboy"
  )
fi
makedepends=()
if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    "termux-shortcuts-utils"
  )
fi
_archive="https://archive.org"
_wikimedia="https://upload.wikimedia.org"
license=(
  "custom"
)
_dmca_exemption="${_archive}/about/dmca.php"
_network=100 # gnosis
_file_system="0x69470b18f8b8b5f92b48f6199dcb147b4be96571" # default file system deployment
_namespace="0x926acb6aA4790ff678848A9F1C59E578B148C786" # that kid address
_evmfs_rom_sum="fee45415c42ff3c1cc1c298af3a830e559153b5651924c8224edf0ef2e62a693"
_pic_sum="0c804b4f962b3a3da35eaad3b84fae29bc6407e3750e59ddf726bd54920a8438"
_evmfs_rom_uri="evmfs://${_network}/${_file_system}/${_namespace}/${_evmfs_rom_sum}"
_evmfs_pic_uri="evmfs://${_network}/${_file_system}/${_namespace}/${_pic_sum}"
source=(
  "${_platform}-template.desktop"
  "launcher"
)
sha256sums=(
  "02f59e1f76a0a4066e0ea2931480e2c7ba60f6f4e74fbbd0ca53358edfb87fed"
  "b25117ba185a5af887b1e14fe8698a4de946475d17998637a88b5c06b58fde44"
)
makedepends+=(
  "evmfs"
)
if [[ ! " ${DLAGENTS[*]} " == *" evmfs::"* ]]; then
  _msg=(
    "no download agent configured to manage"
    "Ethereum Virtual Machine File System"
    "resources (evmfs://<uri>). The download"
    "will happen in the 'prepare' function"
  )
  msg \
    "${_msg[*]}"
  _dl_agent="false"
fi
_rom="${_app_id}.gb.tar.xz::${_evmfs_rom_uri}"
_rom_sum="${_evmfs_rom_sum}"
_pic_uri="${_evmfs_pic_uri}"
if [[ "${_dl_agent}" == "true" ]]; then
  source+=(
    "${_rom}"
    "${_app_id}.png::${_pic_uri}"
  )
  sha256sums+=(
    "${_rom_sum}"
    "${_pic_sum}"
  )
fi

_desktop_file_prepare() {
  msg \
    "preparing desktop file"
  mv \
    "${_platform}-template.desktop" \
    "${_app_id}.desktop"
  sed \
    -i \
    "s/%_title%/${_title}/g" \
    "${_app_id}.desktop"
  sed \
    -i \
    "s/%_pkgdesc%/${pkgdesc}/g" \
    "${_app_id}.desktop"
  sed \
    -i \
    "s/%_app_id%/${_app_id}/g" \
    "${_app_id}.desktop"
  sed \
    -i "s/%_uuid%/${_uuid}/g" \
    "${_app_id}.desktop"
  sed \
    -i \
    "s/%_game_launcher%/${_emulator}/g" \
    "${_app_id}.desktop"
  sed \
    -i \
    "s/%_game_language%/any/g" \
    "${_app_id}.desktop"
  msg \
    "done"
}

_launcher_prepare() {
  msg \
    "preparing command-line launcher"
  mv \
    "launcher" \
    "${pkgname}"
  sed \
    -i \
    "s/%_app_id%/${_app_id}/g" \
    "${pkgname}"
  sed \
    -i \
    "s/%_game_launcher%/${_emulator}/g" \
    "${pkgname}"
  sed \
    -i \
    "s/%_game_language%/any/g" \
    "${pkgname}"
  sed \
    -i \
    "s/%_game_platform%/${_platform}/g" \
    "${pkgname}"
  msg \
    "done"
}

_usr_get() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$(command \
           -v \
	   "env")")"
  dirname \
    "${_bin}"
}

_file_checksum() {
  local \
    _file="${1}" \
    _sum_correct="${2}" \
    _sum
  _checksum_flag="false"
  _sum="$( \
    sha256sum \
      "${_file}" | \
        awk \
          '{print $1}')"
  msg \
    "local file '${_file}' sum: '${_sum}'"
  if [[ "${_sum}" == "${_sum_correct}" ]]; then
    _msg=(
      "local file '$( \
        realpath \
          "${_file}")'"
      "has correct sum."
    )
    msg \
      "${_msg[*]}"
    _checksum_flag="true"
  elif [[ "${_sum}" != "${_rom_sum}" ]]; then
    _msg=(
      "local file '${_file}' sum: '${_sum}'"
      "different from expected '${_rom_sum}'."
    )
    msg \
      "${_msg[*]}"
  fi
}

_evmfs_get() {
  local \
    _file="${1}" \
    _sum="${2}" \
    _uri="${3}" \
    _download \
    _checksum_flag
  _download="false"
  if [[ ! -e "${_file}" ]]; then
    _msg=(
      "file '${_file}'"
      "not found, downloading."
    )
    msg \
      "${_msg[*]}"
    _download="true"
  else
    _file_checksum \
      "${_file}" \
      "${_sum}"
    if [[ "${_checksum_flag}" == "false" ]]; then
      _download="true"
    fi
  fi
  if [[ "${_download}" == "true" ]]; then
    msg \
      "downloading file from evmfs"
    evmfs \
      -v \
      -o \
        "${_file}" \
      get \
        "${_uri}"
  fi
}

prepare() {
  local \
    _sum \
    _download
  _download="false"
  if [[ "${_dl_agent}" == "false" ]]; then
    _evmfs_get \
      "${_app_id}.gb.tar.xz" \
      "${_rom_sum}" \
      "${_evmfs_rom_uri}"
    _evmfs_get \
      "${_app_id}.png" \
      "${_pic_sum}" \
      "${_evmfs_pic_uri}"
  fi
  _desktop_file_prepare
  _launcher_prepare
}

_pkgdir_get() {
  local \
    _pkgdir="${1}"
  realpath \
    "${_pkgdir}/../../../.."
}

package() {
  local \
    _game_dir \
    _rom_dir \
    _rom_install_dir
  _game_dir="/usr/games/${_app_id}"
  install \
    -vdm755 \
    "${pkgdir}${_game_dir}"
  if [[ "${_os}" == "GNU/Linux" ]]; then
    _rom_dir="${_game_dir}"
    _rom_install_dir="${pkgdir}${_rom_dir}"
  elif [[ "${_os}" == "Android" ]]; then
    _rom_dir="/storage/emulated/0/Android/media/${_app_id}"
    _rom_install_dir="$( \
      _pkgdir_get \
        "${pkgdir}")${_rom_dir}"
    ln \
      -s \
      "${_rom_dir}/${_uuid}.gb" \
      "${pkgdir}${_game_dir}/${_uuid}.gb"
  fi
  install \
    -vDm644 \
    "${_app_id}.gb" \
    "${_rom_install_dir}/${_uuid}.gb"
  echo \
    "${_rom_dir}/${_uuid}.gb" > \
    "${pkgdir}${_game_dir}/any"
  install \
    -vDm755 \
    "${_app_id}.desktop" \
    "${pkgdir}/usr/share/applications/${_app_id}.desktop"
  install \
    -vDm644 \
    "${_app_id}.png" \
    "${pkgdir}/usr/share/icons/${_app_id}-${_uuid}.png"
  if [[ "${_os}" == "Android" ]]; then
    install \
      -vdm755 \
      "${pkgdir}/home/.shortcuts"
    termux-shortcut-new \
      -o \
        "${pkgdir}/home/.shortcuts" \
      "${_app_id}.desktop"
    install \
      -vDm644 \
      "${_app_id}.png" \
      "${pkgdir}/home/.shortcuts/icons/${_title}.png"
  fi
  install \
    -vDm755 \
    "${pkgname}" \
    "${pkgdir}/usr/bin/${pkgname}"
}

# vim:set sw=2 sts=-1 et:
