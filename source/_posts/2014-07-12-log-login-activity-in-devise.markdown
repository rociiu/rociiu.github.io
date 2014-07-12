---
layout: post
title: "log login activity in devise"
date: 2014-07-12 09:22:32 +0800
comments: true
categories: 
---

In devise, there's option Trackable to record sign in count and ip, but in some case there's need to save custom logs in database. Turns out you can use Devise hook for that.

Just need to put this under config/initializer/devise.rb.

``` ruby

Warden::Manager.after_set_user except: :fetch do |record, warden, options|
  if record.respond_to?(:update_tracked_fields!) && warden.authenticated?(options[:scope])
    if record.is_a?(User)
      LoginLog.create(user: record, organization: record.organization, ip: warden.request.remote_ip)
    end
  end
end

```
