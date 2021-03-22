# API Document (Version 0.2)

> Any change to API should be record here, **"Readme.md"**;
>
> Change a lot, then we can archive this file as **"API v1.0.md"** etc;
>
> And then continue editing on this readme file.

## 0. Guideline

We are going to use `restful` style API for communication between frontend and backend.

> Reference:
>
> Ruan Yifeng's blog: http://www.ruanyifeng.com/blog/2014/05/restful_api.html
>
> Restful.net: https://restfulapi.net/



## 1. Base URL

```
domainname.com/api/v1/...
```



## 2. API url design

### 2.1 Bus

The bus API is under `/bus/...`

```
GET: /bus/:name/

return a bus's information (according to the bus name).
the timetable is the range time for today (Singapore time). 
maybe only return nearby times.

Example: 
	curl domainname.com/api/v1/bus/D1
    return {
    	id: 1278283,
    	name: D1,
    	route: ['YIH', 'utown', ...],
    	timetable: ['11:20', '11:30', '11:52', ...]
    }
```

```
GET: /bus/:name/route
	return the bus's route
	
Example: 
	curl domainname.com/api/v1/bus/D1/route
    return {
    	['YIH', 'utown', ...]
    }
```

```
GET: /bus/:name/timetable
	return the bus's nearby timetable (range start time)
	
Example: 
	curl domainname.com/api/v1/bus/D1/timetable
    return {
    	['YIH', 'utown', ...]
    }
```



About navigation,

```
GET: /navigation?from=:from&to=:to
	return the avalible bus from :from to :to.
	
Example:
	curl domainname.com/api/v1/navigation?from=FoS&to=utown
	return {
		D1: {
			from: LT27,
			to: utown,
			recent_begin_time: ['11:20', '11:30', ...],
			usual_time_offset: 5,
		},
		D2: {
			from: S17,
			to: utown,
			recent_begin_time: ['11:20', '11:30', ...],
			usual_time_offset: 12,
		},
		...
	}
```



### 2.2 User

User API is under `/user/...`

```
GET: /user/:id
	return user's information(according to the :id)
	
Example:
	curl domainname.com/api/v1/user/12
	return {
		id: 12,
		username: 'TomAndJerry',
		favourates: {
			'bus': ['D1', 'D2', ...],
			'buildings': ['YIH'],
			'activities': [
				{id: 2384814, abstract: ''},
				{id: 5823422, abstract: ''}
			]
		}
	}
```

```
GET: /user/:id/events ? starttime=xxx&endtime=xxx
	return all event belongs to this user(according to the :id)

Example:
	curl domainname.com/api/v1/user/12/events?starttime=2020/12/12T12:00&endtime=..
	return [
        { id: 873421, abstract: 'Do something tomorrow', time: '2020/12/12 12:00' },
        { id: 873428, abstract: 'Eat 2 hamburgers', time: '2020/12/12 15:00' },
		...
	]
```

```
GET: /user/:id/event/:id
	return the :id event of user :id, 

Example:
	curl domainname.com/api/v1/user/12/events/2134243
	return {
		id: 873421, 
		abstract: 'Do something tomorrow', 
		time: '2020/12/12 12:00',
		details: 'Eat all food in utown'
	}
```



```
POST: /user/:id/event
	add a new event for user :id, return the new event
	
Example:
	curl -X POST -F "abstract=\'A\'" -F ... domainname.com/api/v1/events/
	return {
		id: 873421, 
		abstract: 'A', 
		time: '2020/12/12 12:00',
		details: 'Eat all food in utown'
	}
```



### 2.4 Promotion

The Promotion means the business activities, which is under `/promotion/...`

```
GET: /promotion/:id
	return the promotion's information (according to the :id)

Example:
	curl domainname.com/api/v1/promotion/1
	return {
		id: 1, 
		abstract: 'A', 
		time: '2020/12/12 12:00',
		details: 'Eat all food in utown'
		...
	}
```

```
GET: /promotion?building=:build_id ? starttime=xxx&endtime=xxx
	
Example:
	curl domainname.com/api/v1/promotion?building_id=12
	return [
		{ id: 1,  abstract: 'A',  time: '2020/12/12 12:00' },
		...
	]
```



### 2.5 Buildings

```
GET: /building/:id
	return the building's information (according to the :id)

Example:
	curl domainname.com/api/v1/activity?building_id=12
	return { 
		id: 1,  
		name: 'YIH',
		image: 'https://domainname.com/static/images/84231432.jpg',
		story: 'Long long time ago, ...',
		nearest_bus_stops: ['YIH', 'IT', ...]
	}
```



## 3. Static File Server 

Maybe we can use S3 bucket.

### 3.1 Images

```
domainname.com/static/images/:id.jpg
```

### 3.2 Maps

```
domainname.com/static/map
```