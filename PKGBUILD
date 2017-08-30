developer="http://indiecomputing.com/"
url="http://ubos.net/"
maintainer=$developer
pkgname=$(basename $(pwd))
pkgver=0.1
pkgrel=1
pkgdesc="Example blogging app from rubyonrails.org, packaged for UBOS"
arch=('any')
license=('GPL')
options=('!strip')
depends=(
    'ubos-rails-support'
)

build() {
    cd ${startdir}
    RAILS_ENV=production bin/bundle install --deployment --without development test
    RAILS_ENV=production bin/rails assets:precompile db:migrate
}

package() {
# Manifest
    mkdir -p ${pkgdir}/var/lib/ubos/manifests
    install -m0644 ${startdir}/ubos-manifest.json ${pkgdir}/var/lib/ubos/manifests/${pkgname}.json

# Icons
    mkdir -p ${pkgdir}/srv/http/_appicons/${pkgname}
    install -m644 ${startdir}/appicons/{72x72,144x144}.png ${pkgdir}/srv/http/_appicons/${pkgname}/

# Data
    mkdir -p ${pkgdir}/var/lib/${pkgname}

# Code -- be selective in what we package
    mkdir -p ${pkgdir}/usr/share/${pkgname}/
    toCopy=(
        '.bundle'
        'app'
        'bin'
        'config/environments/production.rb'
        'config/initializers'
        'config/locales'
        'config/application.rb'
        'config/boot.rb'
        'config/cable.yml'
        'config/environment.rb'
        'config/routes.rb'
        'config/secrets.yml'
        'config/secrets.yml.key'
        'config/spring.rb'
        'db/migrate'
        'db/schema.rb'
        'db/seeds.rb'
        'lib'
        'public/assets'
        'tmpl'
        'vendor'
        'config.ru'
        'Gemfile'
        'Gemfile.lock'
        'Rakefile'
    )
    for t in ${toCopy[@]}; do
        from="${startdir}/${t}"
        if [[ -e ${from} ]]; then
            to="${pkgdir}/usr/share/${pkgname}/${t}"
            todir=$(dirname $to)
            [[ -d ${todir} ]] || mkdir -p -m 755 ${todir}
            cp -a ${from} ${todir}/
        fi
     done

# Logging
    mkdir -p -m755 ${pkgdir}/var/log/${pkgname}
}
