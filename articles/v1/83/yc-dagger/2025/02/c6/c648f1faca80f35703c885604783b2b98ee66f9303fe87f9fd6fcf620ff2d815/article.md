---
schema_version: "1.0.0"
document_id: "c648f1faca80f35703c885604783b2b98ee66f9303fe87f9fd6fcf620ff2d815"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/php-sdk/"
published_at: "2025-02-18T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:8ffc7e3c8475afefb1b8de4b6c48002ddadf6ede4a151686c0ff9427eb8ed6a1"
---

# Announcing the Dagger PHP SDK: A New Way to Automate with PHP

At Dagger, our mission is to empower developers to define powerful, reusable workflows as code — directly in the programming languages they love. Today, we’re excited to introduce the Dagger PHP Community SDK, created by and for the PHP developer community!


```text
<?php


declare(strict_types=1);


namespace DaggerModule;


use Dagger\Attribute\DaggerFunction;
use Dagger\Attribute\DaggerObject;
use Dagger\Attribute\DefaultPath;
use Dagger\Attribute\Doc;
use Dagger\Container;
use Dagger\Directory;


use function Dagger\dag;


#[DaggerObject]
#[Doc('A generated module for LaravelApp functions')]
class LaravelApp
{
#[DaggerFunction]
public function __construct(
#[DefaultPath(".")]
public Directory $source,
) {
}


#[DaggerFunction]
#[Doc('Set up env')]
public function setupPhpEnv(): Container
{
return dag()
->container()
->from('php:8.4-cli')
// Extensions
->withExec($this->cmd('apt-get update'))
->withExec($this->cmd('apt-get install --yes git-core zip curl'))
// Composer
->withFile(
'/usr/bin/composer',
dag()->container()->from('composer:2')->file('/usr/bin/composer')
)
// App Source
->withMountedDirectory('/app', $this->source)
->withWorkdir('/app')
// Install Deps
->withExec($this->cmd('composer install'));
}


#[DaggerFunction]
#[Doc('Run phpstan')]
public function phpstan(): Container
{
return $this->setupPhpEnv()
->withExec($this->cmd('./vendor/bin/phpstan -v analyse --level=6 ./app'));
}


#[DaggerFunction]
#[Doc('Run phpunit')]
public function tests(): Container
{
return $this->setupPhpEnv()
->withExec($this->cmd('./vendor/bin/phpunit'));
}


private function cmd(string $cmd)
{
return ["/bin/sh", "-c", $cmd];
}
}
```


#### Why PHP Developers Will Love the Dagger SDK


If you’re working with PHP, you likely deal with automation challenges—whether it’s building and shipping applications, managing cloud infrastructure, or automating development workflows. The Dagger PHP SDK brings the flexibility of Dagger’s API to PHP, allowing you to:


- **Define automation workflows in PHP** – No need to switch languages just to script tasks.
- **Run your workflows locally or remotely with full reproducibility** – Test and iterate faster.
- **Easily compose and reuse logic across projects** – Keep your automation modular and maintainable.
- **Leverage Dagger’s caching and portability** – Speed up workflows and run them anywhere.
- **Share content with every Dagger user** –[All Dagger modules](https://daggerverse.dev/) , in any language, can be used together to build your internal platform.


#### Built by the Community, for the Community


The PHP SDK is a[Community SDK](https://dagger.io/community-sdks) , which means it’s maintained by passionate Dagger users who have taken the initiative to bring Dagger’s power to PHP developers. A huge thank you to Paul Dragoonis, John Charman, and Chris Riley for making this happen! Their dedication to open-source innovation is what makes Dagger’s ecosystem thrive.


#### Get Started with the PHP SDK


Ready to automate with PHP? Here’s how to get started:


1. Check out the[PHP SDK on GitHub](https://github.com/dagger/dagger/tree/main/sdk/php)
2. Follow the[Dagger Quickstart](https://docs.dagger.io/quickstart) (now with PHP examples!)
3. Join the Dagger Community on[Discord](https://discord.gg/FXGEhB4vhX)


#### What’s Next?


The PHP SDK is just the beginning! As a[Community SDK](https://dagger.io/community-sdks) , it will continue to evolve with the help of contributors like you. We can’t wait to see what you build with it.


Have feedback? Questions? Share your thoughts in our[community forum](https://discord.gg/dagger) or[GitHub discussions](https://github.com/dagger/dagger/discussions) .


Happy coding! 💙
