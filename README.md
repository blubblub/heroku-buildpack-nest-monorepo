#  NestJS Monorepo Heroku Buildpack

You have a single codebase with multiple NestJS apps as defined in: https://docs.nestjs.com/cli/monorepo

How do you run each on Heroku? You don't. Heroku applications assume one repo to one application.

Enter the Monorepo buildpack, which is a copy of [heroku-buildpack-multi-procfile](https://github.com/heroku/heroku-buildpack-multi-procfile) except it moves the target path in to the root, rather than just the Procfile. This helps for ruby apps etc.

# Usage

1. Write a single `Procfile` and enter your command: `web: npm run start:prod`
2. Create a bunch of Heroku apps.
3. For each app set `NEST_APP=app name`
4. For each app run `heroku buildpacks:add -a <app> https://github.com/blubblub/heroku-buildpack-nest-monorepo`
4. For each app `git push git@heroku.com:<app> main`

# Notes

- **If you already have other buildpacks defined, you'll need to make sure that the heroku-buildpack-nest-monorepo
 buildpack is defined first. You can do this by adding `-i 1` to the `heroku buildpacks:add` command.**
- The buildpack works only on Linux and it depends on `jq` being installed (by default already on Heroku-20 stack)
- It works by updating package.json and appending your NestJS app to commands. Currently it only updates `build`, `start` and `start:prod` commands.

# Author

Dal Rupnik <legoless@gmail.com>
