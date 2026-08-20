---
schema_version: "1.0.0"
document_id: "7c095ebff96c15c4b12cd70c3aef9e4a35a01be19b6b2320fb718458a4766298"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/swiftui-previews-in-statically-linked-ios-apps-w-cocoapods-eb63fa1b0bc1"
published_at: "2023-05-05T20:29:25+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:05fe6c54d609663af2b08ae867b5c9160a59d6387845f37bbe4785d84f029be1"
---

# SwiftUI Previews in statically-linked iOS apps w/Cocoapods

IOS


Swiftui


Apple


Xcode


Cocoapods


# SwiftUI Previews in statically-linked iOS apps w/Cocoapods


[Omar Zuñiga Lagunas](https://medium.com/@omarlagunas?source=post_page---byline--eb63fa1b0bc1---------------------------------------)


5 min read


·


May 5, 2023


--


Press enter or click to view image in full size


## Static linking, why?


At Rappi, we link our code statically to reduce our users’ startup time. Of course, this comes at the cost of incrementing linking time during development.


The purpose of this article is not to discuss the differences between static vs. dynamic linking — there are plenty of resources on the internet about that topic. This article is targeted to those apps that can’t use SwiftUI previews because they use static linking (and won’t change to dynamic linking) and their dependency manager is Cocoapods.


## What can we do?


If you get to this article you might know by now that it is not possible to use statically linked libraries/frameworks with SwiftUI Previews. Apple made a great job implementing the Previews but they extensively use the dynamic capabilities of Swift language runtime to prevent recompiling, you can read more about Behind SwiftUI Previews[here](https://www.guardsquare.com/blog/behind-swiftui-previews) .


So by design, the “hot reload” we are all excited about requires dynamic features, there’s nothing we can do about it. Therefore there is only one possible solution: linking dynamically during development to take advantage of SwiftUI Previews and linking statically in production.


The simplest solution would be to change all our targets to dynamic linking, but we have some edge cases to consider. We have transitive 3rd party dependencies that are static XCFrameworks which wouldn’t be easy to migrate to dynamic since we do not control them. And even if you set` use_frameworks!` in the` Podfile` the` Pods-xxx` target will always be a **static** library or framework by[design](https://github.com/CocoaPods/CocoaPods/blob/master/lib/cocoapods/installer/analyzer.rb#L523) .


Press enter or click to view image in full size


## Cocoapods extensions


There a some modifications required to be done so the previews can find the linker runpath search paths, you can read more about this issue[here](https://github.com/CocoaPods/CocoaPods/issues/9275) .


```text
module Pod    class Target      class BuildSettings::PodTargetSettings        alias ld_runpath_search_paths_original ld_runpath_search_paths               def ld_runpath_search_paths            return ld_runpath_search_paths_original unless target.swiftui_previews_enabled            (ld_runpath_search_paths_original || []) + framework_search_paths        end      end    end  end
```


We also found out a missing symbol error with Xcode 14.x in our apps. We solved it by adding the explicit transitive framework dependencies to the linker flags.


```text
module Pod     class Target       class BuildSettings::PodTargetSettings       alias_method :other_ldflags_original, :other_ldflags             def other_ldflags          return other_ldflags_original unless target.swiftui_previews_enabled          new_flags = other_ldflags_original.dup          dependent_targets_to_link.each do |dep|              dep.framework_paths.each do |_key, fwk_paths|                  fwk_paths.each do |fwk_path|                      name = File.basename(fwk_path.source_path, '.*')                      new_flags << '-framework' << %("#{name}") unless frameworks.include?(name)                  end              end          end          new_flags        end      end    end  end
```


Finally, we tried to keep our build configurations as similar as possible to the production ones. We chose to modify only the targets that contain SwiftUI previews in the code, and the dependencies and transitive dependencies of them.


```text
require 'set'   module Pod    class Installer      class Analyzer        alias original_generate_pod_targets generate_pod_targets         def generate_pod_targets(resolver_specs_by_target, target_inspections)          targets = original_generate_pod_targets(resolver_specs_by_target, target_inspections)           start_time = Time.now          pods_to_modify = Set.new          targets.each do |target|            next unless target.should_enable_swiftui_previews            pods_to_modify << target            pods_to_modify += target.recursive_dependent_targets          end          pods_to_modify.each { |pod| pod.enable_swiftui_previews }          puts "SwiftUI setup #{(Time.now.to_f - start_time.to_f).to_i}s"          targets        end      end    end     class PodTarget       attr_reader :swiftui_previews_enabled       def should_enable_swiftui_previews        # Enabled only for development pods        return unless sandbox.local?(pod_name)        # If it is already a dynamic framework we will return true so the        # linker search paths are modified too        return true if @build_type == Pod::BuildType.dynamic_framework         chunk_size = 512        file_accessors.any? do |fa|          fa.spec.library_specification? &&            fa.source_files.any? do |source|              next if source.extname != '.swift'              begin                file = File.new(source)                data = file.read(chunk_size)                # Source files can be empty                next if data.nil?                # Validates if file imports SwiftUI in the first chunk                next unless data.include?('import SwiftUI')                 validation = ->(data) { data.include?('PreviewProvider') }                break true if validation.call(data)                 # Continues reading the file to validate if it contains a                # Preview Provider                while (new_data = file.read(chunk_size))                  data += new_data                  is_valid = true if validation.call(data)                end                break true if is_valid              ensure                file.close              end              false            end        end      end       def enable_swiftui_previews        @swiftui_previews_enabled = true        @build_type = Pod::BuildType.dynamic_framework      end    end  end
```


In summary, we discarded the immediate solution of using` use_frameworks!` since:


- We have transitive dependencies with compiled static XCFrameworks.
- The` Pods-xxx` target is an static library.
- We could have issues by having production build setting and local build settings. We are modifying only the required targets to make SwiftUI Previews work, nothing more.


## Demo


You can download a working project from this[repo](https://github.com/omarzl/CocoapodsSwiftUIPreviews) .


## Get Omar Zuñiga Lagunas’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Please open the` DemoView` file in the` SecondPod` target and the SwiftUI Previews should be working (tested with Xcode 14.2):


Press enter or click to view image in full size


As you may notice, we configured the` SecondPod` target to be a dynamic framework since it import the SwiftUI module and it uses the protocol` PreviewProvider` in the` DemoView` code.


Press enter or click to view image in full size


You can also see that the` ThirdPod` target doesn’t contain SwiftUI code at all, but since it is a direct dependency of` SecondPod` it will be compiled as a dynamic framework too. Also take a look to the` FourthPod` target, since it is a transitive dependency of` SecondPod` it will also be compiled as a dynamic framework.


If you want to test the compilation with static linking, as if you were to ship the app to production, run` CI=true pod install` . It will change all the targets to static libraries.


Of course, if you try to use SwiftUI Previews it will give you now the following error:


```text
UnsupportedProductTypeError: Previews not supported for SecondPod   Previews can be defined in applications, frameworks, Swift packages, or dynamic libraries, but not in static libraries   productType = com.apple.product-type.library.static  buildable = libSecondPod.a
```


This is how we enabled SwiftUI support in our[modular dev-apps](https://engineering.rappi.com/improving-ios-developer-productivity-and-swiftui-adoption-at-rappi-4a38d3661c53) , this solution let our developers create new UI screens in a faster way since the project only compiles the feature they are working on and with the full support of Previews.
