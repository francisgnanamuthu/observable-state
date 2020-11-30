# observable-state
Angular Observable and State management


### Service 
```
import {Injectable} from '@angular/core';
import {HttpClient} from '@angular/common/http';
import {forkJoin, Observable, of} from 'rxjs';
import {SpeciesResponse} from './model/species-response';
import {StarshipResponse} from './model/starshipResponse';
import {map, mergeMap} from 'rxjs/operators';
import {People} from './model/people';


@Injectable({
  providedIn: 'root'
})
export class SwapAPI {

  const;

  constructor(private readonly httpClient: HttpClient) {
  }

  public getStarWars(): Observable<any> {
    return this.httpClient.get<People>('https://swapi.dev/api/people/1').pipe(
      map((value: People) => {
        value.starships[0] = 'https://swapi.dev/api/starships/12/';
        value.species[0] = 'https://swapi.dev/api/species/4/';
        return value;
      }),
      mergeMap(
        (people: People) => {
          console.log(people.species[0]);
          console.log(people.starships[0]);
          const speciesAPI = this.getSpeciesAPI(people.species[0]);
          const starShipAPI = this.getStarShips(people.starships[0]);
          return forkJoin([speciesAPI, starShipAPI]);
        }), map(
        (value) => value[1]
      )
    );
  }

  public getSpeciesAPI(url: string): Observable<SpeciesResponse> {
    if (url) {
      return this.httpClient.get<SpeciesResponse>(url);
    }
    const resp: SpeciesResponse = {
      average_height: '100'
    };
    return of(resp);
  }

  public getStarShips(url: string): Observable<StarshipResponse> {
    return this.httpClient.get<StarshipResponse>(url);
  }

  joinWithVariableRequests$ = (...args) => {
    return forkJoin(args.map(e => fetch(e).then(eq => eq.json())));
  }
}

```

