# Multi Dimensional Configuration

Configuration is simple when you have it defined in a chunk of code.

    var cfg = require('mdc').create();

    cfg.set({
        title: 'Github',
        host: 'http://github.com'
    });

    cfg.get('title'); // Github

It gets a bit unwieldy but still usable when you introduce different dimensions to your configuration such as _development_ vs _production_.

    var cfg = require('mdc').create([
        {
            "env": {
                "dev": null,
                "prod": null
            }
        }
    ]);

    cfg.set({port: 8080}); // default
    cfg.set({port: 3000}, {env: 'dev'});
    cfg.set({port: 80}, {env: 'prod'});

    cfg.get('port'); // 8080
    cfg.get('port', {env: 'dev'}); // 3000
    cfg.get('port', {env: 'prod'}); // 80

Then there's a whole world of crazy when you start changing things based on _location_, _language_ or _device_ type!

    /*
     * Dimensions to work with
     */
    
    var cfg = require('../').create([
        {
            "env": {
                "dev": null,
                "qa": null,
                "prod": null
            }
        },
        {
            "device": {
                "phone": {
                    "iphone": {
                        "itouch": null
                    },
                    "nexus": null
                },
                "desktop": {
                    "small": null,
                    "large": null
                },
                "tablet": {
                    "ipad": null,
                    "galaxy": null
                }
            }
        },
        {
            "lang": {
                "en": {
                    "en_US": {
                        "en_CA": null
                    },
                    "en_GB": null
                }
            }
        }
    ]);
    
    // Default values

    cfg.set({
        title: "Crazy World!",
        more: "Show more",
        source: "http://world.news.com/",
        show: 10
    });

    // Special values

    cfg.set({
        title: "Crazy world, eh!"
    }, {
        lang: "en_CA"
    });

    cfg.set({
        title: "It's a crazy world out there!"
    }, {
        lang: "en_GB"
    });

    cfg.set({
        source: "http://localhost/test/news/"
    }, {
        env: "dev"
    });

    cfg.set({
        source: "http://qa.my-company.com/news/fixtures/"
    }, {
        env: "qa"
    });

    cfg.set({
        show: 5 // because it's smaller
    }, {
        device: "phone"
    });

    cfg.set({
        show: 15 // because it's larger
    }, {
        device: "desktop"
    });

    cfg.set({
        title: "It's crazy!" // because it's smaller
    }, {
        device: "phone",
        lang: "en_GB"
    });

    cfg.get('source', {env: 'dev'}); // http://localhost/test/news/
    cfg.get('title', {env: 'qa', lang: 'en_GB'}); // It's a crazy world out there!
    cfg.get('show', {env: 'prod', lang: 'en_GB', device: 'itouch'}); // 5
    cfg.get('title', {device: 'large', lang: 'en_CA'}); // Crazy world, eh!

