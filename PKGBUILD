# Maintainer: Claudio Kozicky <claudiokozicky@gmail.com>

pkgname=oilrush-hib
pkgver=1.35
_pkgver=1370041755
pkgrel=1
pkgdesc="A naval tower defense RTS game that takes place in a world where nuclear war has melted the ice caps."
arch=(i686 x86_64)
url="http://oilrush-game.com/"
license=(custom)
depends=(libgl openal qt4 qtwebkit)
provides=oilrush
options=(!strip)
source=(hib://OilRush_"$pkgver"_Linux_$_pkgver.run
        hib://oil_rush_tower_defense_map_pack.zip
        $pkgname.desktop)
md5sums=(5a38d0545a64e0d21ddfe4afc15253a8
         6c0207e1d75b281a2e001446756ab37e
         fa29db8a94687d32a9eec78bf69371e9)
DLAGENTS+=('hib::/usr/bin/echo Could not find %u. Manually download it to \"$(pwd)\", or set up a hib:// DLAGENT in /etc/makepkg.conf.; exit 1')
PKGEXT=.pkg.tar

prepare()
{
    cd "$srcdir"
    sh OilRush_"$pkgver"_Linux_$_pkgver.run --nox11
    mv OilRush-$pkgver/launcher.sh .
}

package()
{
    # data
    cd "$srcdir"/OilRush-$pkgver
    install -d "$pkgdir"/opt/$pkgname
    cp -Rl . "$pkgdir"/opt/$pkgname
    # DLC data
    cd "$srcdir"
    install -m644 oilrush_dlc_td_map_pack.ung "$pkgdir"/opt/$pkgname/data

    # launcher
    sed "s:cd ./bin:cd /opt/oilrush-hib/bin:" -i launcher.sh
    install -Dm755 launcher.sh "$pkgdir"/usr/bin/$pkgname

    # desktop integration
    install -d "$pkgdir"/usr/share/pixmaps
    ln -s /opt/$pkgname/data/launcher/oilrush.png "$pkgdir"/usr/share/pixmaps/$pkgname.png
    install -Dm644 $pkgname.desktop "$pkgdir"/usr/share/applications/$pkgname.desktop

    # cleanup
    cd "$pkgdir"/opt/$pkgname/bin
    rm libopenal.so.1
    [ $CARCH = i686 ] \
        && rm *x64* \
        && for i in Core Gui Network WebKit Xml
           do
               ln -sf /usr/lib/libQt"$i".so.4 "$pkgdir"/opt/$pkgname/bin/libQt"$i"Unigine_x86.so.4
           done \
        || true
    [ $CARCH = x86_64 ] \
        && rm *x86* \
        && for i in Core Gui Network WebKit Xml
           do
               ln -sf /usr/lib/libQt"$i".so.4 "$pkgdir"/opt/$pkgname/bin/libQt"$i"Unigine_x64.so.4
           done \
        || true
}

# vim:et:sw=4:sts=4
