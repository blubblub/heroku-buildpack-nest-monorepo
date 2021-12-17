#  NestJS Monorepo Heroku Buildpack

You have a single codebase with multiple NestJS apps as defined in: https://docs.nestjs.com/cli/monorepo

How do you run each on Heroku? You don't. Heroku applications assume one repo to one application.

Enter the NestJS Monorepo buildpack, which updates package.json in the root of your NestJS project by adding the defined app to the `scripts`.

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
- If you change the environment variable `NEST_APP`, you will have to redeploy, as Heroku does not run buildpack in this case.

# Author

Dal Rupnik <legoless@gmail.com>
