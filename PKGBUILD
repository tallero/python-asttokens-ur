# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
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
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

_pkg=asttokens
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
pkgname="${_py}-${_pkg}"
pkgver=3.0.0
pkgrel=1
_pkgdesc=(
  'Get the currently executing AST'
  'node of a frame, and other information'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_http="https://github.com"
_ns="gristlabs"
url="${_http}/${_ns}/${_pkg}"
license=(
  "Apache-2.0"
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  "git"
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools-scm"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-astroid"
  "${_py}-pytest"
)
source=(
  "git+${url}.git#tag=v${pkgver}"
)
b2sums=(
  '553fb87e6a611e954d67a7312d8df2d5c6ca152368579b5107e36ee9b351c901ca295c0ab6e0cf05a74e35f06a544d32f3969ef8dfdf1f3352b9d614e35d37bf'
)

build() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      "build" \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      "venv" \
    --system-site-packages \
    "test-env"
  "test-env/bin/${_py}" \
    -m \
      "installer" \
    "dist/"*".whl"
  "test-env/bin/${_py}" \
    -m \
      "pytest"
}

package() {
  local \
    _site_packages
  _site_packages="$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")"
  install \
    -d \
    "${pkgdi}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      "installer" \
    --destdir="${pkgdir}" \
    "dist/"*".whl"
}
